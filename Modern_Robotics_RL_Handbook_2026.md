# Modern Robotics RL Handbook 2026
## MuJoCo/MJX Ecosystem for Humanoids, Quadrupeds & Dexterous Hands

---

## 1. The 2026 MuJoCo Stack

### Core Engine Comparison

| Feature | **MuJoCo 3.x (CPU)** | **MJX (JAX/GPU)** | **Brax v2** |
|---------|---------------------|-------------------|-------------|
| **Backend** | Native C, multithreaded | JAX XLA compilation | JAX XLA compilation |
| **Parallelization** | ~64 envs (CPU threads) | **4096-32768 envs** (GPU) | 4096-65536 envs (GPU) |
| **Contact Solver** | Newton + PGS | Convex optimization | Spring-based / PBD |
| **Sim Fidelity** | **Gold standard** | ~98% parity w/ MuJoCo | Lower (tuned for speed) |
| **Gradient Support** | Finite diff only | **Native autodiff** | Native autodiff |
| **Sim-to-Real** | **Excellent** | **Excellent** | Moderate (requires tuning) |
| **Soft contacts** | Full support | Full support | Limited |
| **Muscle actuators** | Full support | Partial | No |

### Library Ecosystem

| Library | Purpose | GPU Support | Best For |
|---------|---------|-------------|----------|
| **mujoco** (dm-mujoco) | Core physics API | ❌ CPU | Ground-truth sim, debugging |
| **mujoco-mjx** | JAX-compiled MuJoCo | ✅ TPU/GPU | Massive parallel training |
| **mujoco-playground** | Pre-built envs + baselines | ✅ MJX | Quick prototyping, benchmarks |
| **loco-mujoco** | Locomotion-specific envs | ❌ CPU | Imitation learning, MoCap |
| **gymnasium[mujoco]** | RL interface standard | ❌ CPU | Single-env, Stable-Baselines3 |
| **dm_control** | DeepMind task suite | ❌ CPU | Benchmarking, complex tasks |
| **robosuite** | Manipulation tasks | ❌ CPU | Robot arm manipulation |

### Recommended Stack (2026)

```
Training Pipeline:       MJX + mujoco-playground + cleanrl/purejaxrl
Sim-to-Real Validation:  MuJoCo CPU (gold standard)
Deployment:              MuJoCo C API (real-time control)
```

---

## 2. Essential XML (MJCF) Design

### Joint Configuration Best Practices

```xml
<mujoco model="humanoid_v2">
  <compiler angle="radian" autolimits="true"/>
  
  <default>
    <!-- CRITICAL: Realistic damping prevents oscillation -->
    <joint damping="0.5" armature="0.01" frictionloss="0.1"/>
    <geom friction="1.0 0.005 0.0001" condim="3" contype="1" conaffinity="1"/>
    <motor ctrlrange="-1 1" ctrllimited="true"/>
  </default>

  <worldbody>
    <body name="torso" pos="0 0 1.2">
      <!-- Floating base: 6-DOF for humanoids/quadrupeds -->
      <freejoint name="root"/>
      
      <body name="thigh_r" pos="0.1 0 -0.2">
        <!-- Hip: 3-DOF ball joint decomposed for control -->
        <joint name="hip_x_r" type="hinge" axis="1 0 0" range="-0.5 1.5"/>
        <joint name="hip_y_r" type="hinge" axis="0 1 0" range="-0.4 0.4"/>
        <joint name="hip_z_r" type="hinge" axis="0 0 1" range="-0.8 0.3"/>
        
        <!-- Knee: High stiffness, realistic limits -->
        <body name="shin_r" pos="0 0 -0.4">
          <joint name="knee_r" type="hinge" axis="0 1 0" 
                 range="0.05 2.5" stiffness="0" damping="2.0"/>
        </body>
      </body>
    </body>
  </worldbody>
</mujoco>
```

### Actuator Selection Matrix

| Control Mode | MJCF Tag | Use Case | Sim-to-Real | Bandwidth |
|--------------|----------|----------|-------------|-----------|
| **Position (PD)** | `<position>` | Servo motors, safe exploration | ⭐⭐⭐ Easy | Low |
| **Velocity** | `<velocity>` | Wheeled robots, conveyors | ⭐⭐ Moderate | Medium |
| **Torque (Direct)** | `<motor>` | High-performance, whole-body | ⭐ Difficult | **High** |
| **Muscle** | `<muscle>` | Biomechanics research | ⭐⭐ Moderate | Biological |

```xml
<actuator>
  <!-- Position Control: Best for Sim-to-Real beginners -->
  <position name="hip_servo" joint="hip_x_r" 
            kp="100" kv="10" 
            ctrlrange="-1.57 1.57" forcerange="-50 50"/>
  
  <!-- Torque Control: Maximum performance, harder transfer -->
  <motor name="hip_torque" joint="hip_x_r" 
         gear="50" ctrlrange="-1 1" forcerange="-100 100"/>
  
  <!-- Integrated Position + Velocity (2026 best practice) -->
  <general name="hip_impedance" joint="hip_x_r" 
           gaintype="fixed" biastype="affine"
           gainprm="100" biasprm="0 -100 -10"/>
</actuator>
```

### Sensor Configuration for RL

```xml
<sensor>
  <!-- IMU Suite (Critical for balance) -->
  <accelerometer name="imu_acc" site="torso_imu"/>
  <gyro name="imu_gyro" site="torso_imu"/>
  <framequat name="torso_quat" objtype="site" objname="torso_imu"/>
  
  <!-- Proprioception -->
  <jointpos name="joint_pos" joint="hip_x_r"/>
  <jointvel name="joint_vel" joint="hip_x_r"/>
  <actuatorfrc name="motor_torque" actuator="hip_torque"/>
  
  <!-- Contact/Force-Torque (Feet/Hands) -->
  <touch name="foot_r_contact" site="foot_r_site"/>
  <force name="foot_r_force" site="foot_r_site"/>
  <torque name="foot_r_torque" site="foot_r_site"/>
  
  <!-- Tactile Array (Dexterous Hands) -->
  <touch name="thumb_tip" site="thumb_tip_site"/>
  <touch name="index_tip" site="index_tip_site"/>
</sensor>
```

### Key MJCF Parameters Reference

| Parameter | Location | Effect | Typical Range |
|-----------|----------|--------|---------------|
| **damping** | `<joint>` | Viscous resistance | 0.1 - 5.0 Nm·s/rad |
| **armature** | `<joint>` | Rotor inertia | 0.001 - 0.1 kg·m² |
| **frictionloss** | `<joint>` | Dry friction torque | 0.0 - 1.0 Nm |
| **stiffness** | `<joint>` | Spring constant | 0 - 1000 N/m |
| **solref** | `<geom>` | Contact time constant | [0.02, 1.0] |
| **solimp** | `<geom>` | Contact impedance | [0.9, 0.95, 0.001] |
| **condim** | `<geom>` | Contact dimensions | 1, 3, 4, or 6 |
| **impratio** | `<option>` | Impedance ratio | 1.0 - 10.0 |

---

## 3. The "Reward Engineering" Matrix

### Humanoid Locomotion

| Component | Formula | Weight | Purpose |
|-----------|---------|--------|---------|
| **Velocity Tracking** | `-\|v_{cmd} - v_{actual}\|^2` | 1.0 - 2.0 | Primary objective |
| **Uprightness** | `torso_z · [0,0,1]` | 0.5 - 1.0 | Prevent falling |
| **Head Height** | `h_{head} / h_{nominal}` | 0.2 - 0.5 | Stable posture |
| **Energy Efficiency** | `-\sum \|\tau_i \cdot \dot{q}_i\|` | 0.01 - 0.1 | Reduce torque |
| **Action Smoothness** | `-\|a_t - a_{t-1}\|^2` | 0.05 - 0.2 | Smooth motion |
| **Joint Limit Penalty** | `-\sum \max(0, \|q\| - q_{lim})^2` | 1.0 - 10.0 | Safety |
| **Foot Clearance** | `\min(z_{swing}, 0.05) / 0.05` | 0.1 - 0.3 | Avoid scuffing |
| **Termination Penalty** | `-100` on fall | Fixed | Discourage failure |

```python
def humanoid_reward(data, cmd_vel, prev_action, action):
    # Core rewards
    vel_reward = -jnp.sum((cmd_vel - data.qvel[:3])**2)
    upright = data.xmat[1, 8]  # torso z-axis alignment
    head_height = data.xpos[2, 2] / 1.6  # normalized
    
    # Penalties
    energy = -0.01 * jnp.sum(jnp.abs(data.qfrc_actuator * data.qvel[6:]))
    smoothness = -0.1 * jnp.sum((action - prev_action)**2)
    
    # Contact reward (both feet should contact during stance)
    feet_contact = jnp.sum(data.contact.geom[:, 0] == foot_geom_ids)
    
    return vel_reward + 0.5*upright + 0.3*head_height + energy + smoothness
```

### Quadruped Locomotion

| Component | Formula | Weight | Purpose |
|-----------|---------|--------|---------|
| **Velocity Tracking** | `-\|v_{cmd} - v_{xy}\|^2 - 0.5\|\omega_{cmd} - \omega_z\|^2` | 1.5 | Track commands |
| **Gait Symmetry** | `cos(2\pi \cdot phase_{diag})` | 0.3 - 0.5 | Enforce trot/walk |
| **Contact Schedule** | `\sum_{legs} c_i \cdot s_i(t)` | 0.2 - 0.4 | Penalize mistimed contact |
| **Base Stability** | `-\|\omega_{roll,pitch}\|^2` | 0.5 | Reduce body oscillation |
| **Foot Slip** | `-\sum \|v_{foot}\| \cdot c_{foot}` | 0.5 - 1.0 | Penalize sliding |
| **Air Time** | `\sum (t_{air} > 0.1)` | 0.1 | Encourage swing phase |
| **Stance Width** | `-(\Delta y_{feet} - y_{nom})^2` | 0.1 | Natural stance |

```python
def quadruped_reward(data, cmd, gait_phase, dt):
    # Velocity tracking (x, y, yaw)
    lin_vel_error = jnp.sum((cmd[:2] - data.qvel[:2])**2)
    ang_vel_error = (cmd[2] - data.qvel[5])**2
    tracking = jnp.exp(-lin_vel_error/0.25) + 0.5*jnp.exp(-ang_vel_error/0.25)
    
    # Gait phase reward (diagonal sync for trot)
    phase_fl_br = jnp.cos(2*jnp.pi*(gait_phase[0] - gait_phase[3]))
    phase_fr_bl = jnp.cos(2*jnp.pi*(gait_phase[1] - gait_phase[2]))
    gait_sync = 0.3 * (phase_fl_br + phase_fr_bl)
    
    # Foot slip penalty
    foot_velocities = get_foot_velocities(data)
    contacts = get_foot_contacts(data)
    slip_penalty = -0.5 * jnp.sum(jnp.linalg.norm(foot_velocities, axis=1) * contacts)
    
    return tracking + gait_sync + slip_penalty
```

### Dexterous Hand Manipulation

| Component | Formula | Weight | Purpose |
|-----------|---------|--------|---------|
| **Object Pose** | `-\|p_{obj} - p_{goal}\|^2 - \lambda\|q_{obj} - q_{goal}\|^2` | 2.0 | Primary goal |
| **Orientation Alignment** | `quat\_similarity(q_{obj}, q_{goal})` | 1.0 - 1.5 | Rotation matching |
| **Contact Richness** | `\sum_{fingers} \mathbb{1}[f_i > 0]` | 0.3 | Multi-finger grasp |
| **Grasp Stability** | `-variance(f_{contacts})` | 0.2 | Even force distribution |
| **Tactile Feedback** | `\sum tactile_i \cdot \mathbb{1}[correct\_contact]` | 0.2 - 0.5 | Reward tactile use |
| **Fingertip Proximity** | `\sum -\|p_{tip} - p_{contact\_point}\|` | 0.3 | Approach phase |
| **Object Velocity Penalty** | `-\|v_{obj}\|^2$ (when grasped)` | 0.5 | Stable manipulation |
| **Drop Penalty** | `-50` if dropped | Fixed | Maintain grasp |

```python
def hand_reward(data, goal_pos, goal_quat, tactile_readings):
    obj_pos = data.xpos[obj_body_id]
    obj_quat = data.xquat[obj_body_id]
    
    # Position and orientation
    pos_error = jnp.linalg.norm(obj_pos - goal_pos)
    quat_error = 1 - jnp.abs(jnp.dot(obj_quat, goal_quat))
    pose_reward = jnp.exp(-10*pos_error) + jnp.exp(-5*quat_error)
    
    # Contact richness: reward multiple finger contacts
    finger_contacts = tactile_readings > 0.01  # threshold
    contact_reward = 0.3 * jnp.sum(finger_contacts) / 5  # normalize by fingers
    
    # Grasp stability: variance of contact forces
    contact_forces = tactile_readings[finger_contacts]
    stability = -0.2 * jnp.var(contact_forces) if contact_forces.size > 1 else 0
    
    return pose_reward + contact_reward + stability
```

---

## 4. Modern Training Architectures

### MJX Parallel Environment Setup

```python
import jax
import jax.numpy as jnp
from mujoco import mjx
import mujoco

# Load model once, compile for GPU
mj_model = mujoco.MjModel.from_xml_path("robot.xml")
mjx_model = mjx.put_model(mj_model)

# Vectorized reset function
@jax.jit
def reset_envs(rng, n_envs=4096):
    rngs = jax.random.split(rng, n_envs)
    # Batch initial states with randomization
    qpos = mjx_model.qpos0 + 0.01 * jax.random.normal(rngs, (n_envs, mjx_model.nq))
    qvel = 0.01 * jax.random.normal(rngs, (n_envs, mjx_model.nv))
    
    # Vectorized mjx.make_data
    data = jax.vmap(lambda q, v: mjx.make_data(mjx_model).replace(qpos=q, qvel=v))(qpos, qvel)
    return data

# Vectorized step function
@jax.jit
def step_envs(data, actions):
    def single_step(d, a):
        d = d.replace(ctrl=a)
        return mjx.step(mjx_model, d)
    return jax.vmap(single_step)(data, actions)
```

### PPO Configuration for Massive Parallelism

| Parameter | 4096 Envs | 8192 Envs | 16384+ Envs |
|-----------|-----------|-----------|-------------|
| **Minibatch Size** | 256-512 | 512-1024 | 1024-2048 |
| **Num Minibatches** | 16-32 | 16-32 | 16-32 |
| **Update Epochs** | 4-8 | 4-6 | 2-4 |
| **Horizon (steps)** | 16-32 | 16-24 | 8-16 |
| **Learning Rate** | 3e-4 | 1e-4 | 5e-5 |
| **Clip Epsilon** | 0.2 | 0.2 | 0.1-0.2 |
| **GAE Lambda** | 0.95 | 0.95 | 0.95-0.97 |
| **Entropy Coeff** | 0.01 | 0.005 | 0.001-0.005 |
| **Value Coeff** | 0.5 | 0.5 | 0.5 |
| **Max Grad Norm** | 0.5 | 0.5 | 0.5 |

```python
# CleanRL-style PPO config for MJX
@dataclass
class PPOConfig:
    # Environment
    num_envs: int = 4096
    num_steps: int = 16          # Horizon per update
    
    # Training
    total_timesteps: int = 1_000_000_000
    learning_rate: float = 3e-4
    anneal_lr: bool = True
    
    # PPO specifics
    gamma: float = 0.99
    gae_lambda: float = 0.95
    num_minibatches: int = 32
    update_epochs: int = 4
    clip_coef: float = 0.2
    clip_vloss: bool = True
    ent_coef: float = 0.01
    vf_coef: float = 0.5
    max_grad_norm: float = 0.5
    target_kl: float = 0.02      # Early stopping
    
    # Network
    hidden_sizes: tuple = (256, 256, 256)
    activation: str = "elu"       # ELU > ReLU for locomotion
    
    # Normalization (CRITICAL)
    normalize_obs: bool = True
    normalize_returns: bool = True
    obs_clip: float = 10.0
```

### SAC Configuration for Sample Efficiency

```python
@dataclass  
class SACConfig:
    # Typically fewer envs for off-policy
    num_envs: int = 256
    buffer_size: int = 1_000_000
    batch_size: int = 256
    
    # SAC specifics
    gamma: float = 0.99
    tau: float = 0.005            # Soft update rate
    learning_rate: float = 3e-4
    alpha: float = 0.2            # Entropy temperature
    autotune_alpha: bool = True   # Automatic entropy tuning
    target_entropy_ratio: float = 0.5
    
    # Update frequency
    learning_starts: int = 10000
    gradient_steps: int = 1       # Per env step
    policy_frequency: int = 2     # Delayed policy update
    
    # Network
    hidden_sizes: tuple = (256, 256)
    activation: str = "relu"
```

### Network Architecture Patterns

```python
import flax.linen as nn

class ActorCriticMLP(nn.Module):
    action_dim: int
    hidden_sizes: tuple = (256, 256, 256)
    
    @nn.compact
    def __call__(self, x, training=True):
        # Shared encoder (optional, often separate is better)
        for size in self.hidden_sizes[:-1]:
            x = nn.Dense(size)(x)
            x = nn.LayerNorm()(x)  # LayerNorm > BatchNorm for RL
            x = nn.elu(x)
        
        # Policy head
        actor = nn.Dense(self.hidden_sizes[-1])(x)
        actor = nn.elu(actor)
        mean = nn.Dense(self.action_dim)(actor)
        log_std = self.param('log_std', nn.initializers.zeros, (self.action_dim,))
        log_std = jnp.clip(log_std, -5, 2)  # Prevent collapse
        
        # Value head
        critic = nn.Dense(self.hidden_sizes[-1])(x)
        critic = nn.elu(critic)
        value = nn.Dense(1)(critic)
        
        return mean, log_std, value

# For dexterous hands: add proprioceptive vs. tactile streams
class MultiModalPolicy(nn.Module):
    @nn.compact
    def __call__(self, proprio, tactile):
        # Proprioceptive stream
        p = nn.Dense(128)(proprio)
        p = nn.elu(p)
        
        # Tactile stream (often needs special treatment)
        t = nn.Dense(64)(tactile)
        t = nn.elu(t)
        t = nn.Dense(64)(t)
        t = nn.elu(t)
        
        # Fusion
        fused = jnp.concatenate([p, t], axis=-1)
        return fused
```

---

## 5. Sim-to-Real Strategy (The Reality Gap)

### Domain Randomization Checklist

#### Dynamics Randomization

| Parameter | Range | Distribution | Priority |
|-----------|-------|--------------|----------|
| **Link Mass** | ±15-30% | Uniform | 🔴 Critical |
| **Link Inertia** | ±20-40% | Uniform | 🔴 Critical |
| **Joint Damping** | ±30-50% | Log-uniform | 🔴 Critical |
| **Joint Friction** | ±50-100% | Log-uniform | 🟡 High |
| **Actuator Strength** | ±10-20% | Uniform | 🔴 Critical |
| **Ground Friction** | 0.5-1.5 | Uniform | 🟡 High |
| **CoM Offset** | ±2-5 cm | Uniform | 🟡 High |
| **Motor Backlash** | 0-0.02 rad | Uniform | 🟢 Medium |

```python
@jax.jit
def randomize_dynamics(rng, mjx_model):
    rngs = jax.random.split(rng, 6)
    
    # Mass randomization
    mass_scale = 1.0 + 0.2 * jax.random.uniform(rngs[0], (mjx_model.nbody,), minval=-1, maxval=1)
    new_mass = mjx_model.body_mass * mass_scale
    
    # Inertia randomization (scale with mass)
    inertia_scale = mass_scale[:, None] * (1.0 + 0.3 * jax.random.uniform(rngs[1], mjx_model.body_inertia.shape, minval=-1, maxval=1))
    new_inertia = mjx_model.body_inertia * inertia_scale
    
    # Damping randomization
    damp_scale = jnp.exp(0.3 * jax.random.uniform(rngs[2], (mjx_model.njnt,), minval=-1, maxval=1))
    new_damping = mjx_model.dof_damping * damp_scale
    
    # Friction randomization
    friction_scale = 1.0 + 0.4 * jax.random.uniform(rngs[3], (mjx_model.ngeom,), minval=-1, maxval=1)
    new_friction = mjx_model.geom_friction.at[:, 0].multiply(friction_scale)
    
    return mjx_model.replace(
        body_mass=new_mass,
        body_inertia=new_inertia,
        dof_damping=new_damping,
        geom_friction=new_friction
    )
```

#### Observation Noise & Latency

| Component | Noise Model | Magnitude | Priority |
|-----------|-------------|-----------|----------|
| **Joint Position** | Gaussian | σ = 0.01-0.02 rad | 🔴 Critical |
| **Joint Velocity** | Gaussian | σ = 0.1-0.5 rad/s | 🔴 Critical |
| **IMU Acceleration** | Gaussian + bias | σ = 0.1 m/s², bias = 0.05 | 🔴 Critical |
| **IMU Gyro** | Gaussian + bias | σ = 0.05 rad/s, bias = 0.01 | 🔴 Critical |
| **Force-Torque** | Gaussian | σ = 5-10% | 🟡 High |
| **Observation Latency** | Uniform | 0-20 ms | 🔴 Critical |
| **Action Latency** | Uniform | 5-30 ms | 🔴 Critical |
| **Action Delay (steps)** | 1-3 steps | Categorical | 🔴 Critical |

```python
@dataclass
class NoiseConfig:
    joint_pos_std: float = 0.01
    joint_vel_std: float = 0.3
    imu_acc_std: float = 0.1
    imu_gyro_std: float = 0.05
    imu_acc_bias_range: float = 0.1
    imu_gyro_bias_range: float = 0.02
    obs_latency_range: tuple = (0, 2)   # steps
    action_latency_range: tuple = (1, 3) # steps

def add_observation_noise(rng, obs, config):
    rngs = jax.random.split(rng, 4)
    
    # Extract observation components
    joint_pos = obs[:n_joints]
    joint_vel = obs[n_joints:2*n_joints]
    imu = obs[2*n_joints:2*n_joints+6]
    
    # Add noise
    joint_pos += config.joint_pos_std * jax.random.normal(rngs[0], joint_pos.shape)
    joint_vel += config.joint_vel_std * jax.random.normal(rngs[1], joint_vel.shape)
    imu += config.imu_acc_std * jax.random.normal(rngs[2], imu.shape)
    
    return jnp.concatenate([joint_pos, joint_vel, imu, obs[2*n_joints+6:]])

# Action delay buffer
class ActionDelayBuffer:
    def __init__(self, max_delay=3, action_dim=12):
        self.buffer = jnp.zeros((max_delay, action_dim))
        
    def __call__(self, action, delay):
        self.buffer = jnp.roll(self.buffer, 1, axis=0)
        self.buffer = self.buffer.at[0].set(action)
        return self.buffer[delay]
```

#### Terrain Randomization (Quadrupeds)

```python
def generate_terrain_heightfield(rng, size=100, resolution=0.02):
    rngs = jax.random.split(rng, 4)
    
    # Base heightfield
    hfield = jnp.zeros((size, size))
    
    # Perlin-like noise at multiple scales
    for scale, amplitude in [(4, 0.1), (8, 0.05), (16, 0.02)]:
        noise = jax.random.uniform(rngs[0], (size//scale, size//scale))
        noise = jax.image.resize(noise, (size, size), method='bilinear')
        hfield += amplitude * noise
    
    # Random stairs
    stair_height = 0.05 + 0.1 * jax.random.uniform(rngs[1])
    stair_regions = jax.random.uniform(rngs[2], (size, size)) > 0.8
    hfield = jnp.where(stair_regions, hfield + stair_height, hfield)
    
    return hfield
```

### System Identification Pipeline

```
┌─────────────────────────────────────────────────────────────┐
│                  System Identification                       │
├─────────────────────────────────────────────────────────────┤
│ 1. HARDWARE DATA COLLECTION                                  │
│    • Record joint positions, velocities, torques            │
│    • Record IMU data at 500Hz+                              │
│    • Record actuator commands vs actual positions           │
│    • Duration: 10-30 minutes diverse motions                │
│                                                             │
│ 2. MOTOR DYNAMICS IDENTIFICATION                            │
│    • Fit Kp, Kd for position-controlled joints              │
│    • Identify torque constant, friction, damping            │
│    • Measure actuator bandwidth (typically 20-50 Hz)        │
│                                                             │
│ 3. RIGID BODY PARAMETERS                                    │
│    • CAD model as initial estimate                          │
│    • Optimize masses/inertias via trajectory matching       │
│    • Validate with drop tests, pendulum tests               │
│                                                             │
│ 4. CONTACT DYNAMICS                                         │
│    • Measure ground reaction forces                         │
│    • Tune solref/solimp in MJCF                            │
│    • Match contact timing and force profiles                │
│                                                             │
│ 5. LATENCY MEASUREMENT                                      │
│    • Round-trip time: command → sensor feedback             │
│    • Typical values: 10-50 ms total                         │
│    • Add to simulation as fixed delay                       │
└─────────────────────────────────────────────────────────────┘
```

---

## 6. MuJoCo Python API Cheat Sheet

### Core Data Structures

```python
import mujoco
import numpy as np

# Load model
model = mujoco.MjModel.from_xml_path("robot.xml")  # From file
model = mujoco.MjModel.from_xml_string(xml_string) # From string

# Create simulation state
data = mujoco.MjData(model)

# Model properties (READ-ONLY after compilation)
model.nq          # Number of position coordinates
model.nv          # Number of velocity coordinates (DOFs)
model.nu          # Number of actuators
model.nbody       # Number of bodies
model.njnt        # Number of joints
model.ngeom       # Number of geometries
model.nsite       # Number of sites
model.nsensor     # Number of sensors
model.opt.timestep # Simulation timestep
```

### State Vectors

```python
# Position state (nq,)
data.qpos[:]      # All positions [free_joint(7) + hinges...]
data.qpos[0:3]    # Root translation (if freejoint)
data.qpos[3:7]    # Root quaternion [w, x, y, z] (if freejoint)
data.qpos[7:]     # Joint angles

# Velocity state (nv,)
data.qvel[:]      # All velocities [free_joint(6) + hinges...]
data.qvel[0:3]    # Root linear velocity
data.qvel[3:6]    # Root angular velocity
data.qvel[6:]     # Joint velocities

# Acceleration (nv,) - computed during step
data.qacc[:]      # Accelerations

# Control inputs (nu,)
data.ctrl[:]      # Actuator commands (set before mj_step)
```

### Simulation Stepping

```python
# Single physics step
mujoco.mj_step(model, data)

# Step with control substeps (more accurate)
mujoco.mj_step1(model, data)  # Compute position-dependent quantities
# ... set data.ctrl here ...
mujoco.mj_step2(model, data)  # Compute dynamics

# Typical RL loop
def env_step(model, data, action, n_substeps=4):
    data.ctrl[:] = action
    for _ in range(n_substeps):
        mujoco.mj_step(model, data)
    return get_obs(data), compute_reward(data)

# Forward kinematics only (no dynamics)
mujoco.mj_forward(model, data)

# Reset simulation
mujoco.mj_resetData(model, data)
data.qpos[:] = initial_qpos
data.qvel[:] = initial_qvel
mujoco.mj_forward(model, data)  # Recompute derived quantities
```

### Accessing Body/Geom/Site Data

```python
# Get IDs by name
body_id = mujoco.mj_name2id(model, mujoco.mjtObj.mjOBJ_BODY, "torso")
geom_id = mujoco.mj_name2id(model, mujoco.mjtObj.mjOBJ_GEOM, "floor")
site_id = mujoco.mj_name2id(model, mujoco.mjtObj.mjOBJ_SITE, "imu_site")
joint_id = mujoco.mj_name2id(model, mujoco.mjtObj.mjOBJ_JOINT, "hip_x")

# Body poses (after mj_forward or mj_step)
data.xpos[body_id]    # Position in world frame (3,)
data.xquat[body_id]   # Orientation quaternion [w,x,y,z] (4,)
data.xmat[body_id]    # Rotation matrix (9,) flattened row-major

# Site poses
data.site_xpos[site_id]  # Position (3,)
data.site_xmat[site_id]  # Rotation matrix (9,)

# Velocities
data.cvel[body_id]       # Cartesian velocity [rot(3), lin(3)]
data.subtree_linvel[body_id]  # CoM velocity of subtree
```

### Contact Information

```python
# Number of active contacts
n_contacts = data.ncon

# Iterate contacts
for i in range(data.ncon):
    contact = data.contact[i]
    geom1 = contact.geom1    # First geom in contact
    geom2 = contact.geom2    # Second geom
    pos = contact.pos        # Contact point position (3,)
    frame = contact.frame    # Contact frame (9,)
    dist = contact.dist      # Penetration depth
    
    # Contact force (6,) - [normal, tangent1, tangent2, torque...]
    force = np.zeros(6)
    mujoco.mj_contactForce(model, data, i, force)
```

### Sensor Readings

```python
# All sensor data concatenated
data.sensordata[:]

# Access by sensor ID
sensor_id = mujoco.mj_name2id(model, mujoco.mjtObj.mjOBJ_SENSOR, "imu_acc")
sensor_adr = model.sensor_adr[sensor_id]  # Start index
sensor_dim = model.sensor_dim[sensor_id]  # Dimensions
reading = data.sensordata[sensor_adr:sensor_adr + sensor_dim]
```

### Jacobians and Dynamics

```python
# Compute Jacobian for a body point
jacp = np.zeros((3, model.nv))  # Positional Jacobian
jacr = np.zeros((3, model.nv))  # Rotational Jacobian
point = np.array([0, 0, 0])      # Point in body frame
mujoco.mj_jac(model, data, jacp, jacr, point, body_id)

# Full dynamics matrices
M = np.zeros((model.nv, model.nv))  # Mass matrix
mujoco.mj_fullM(model, data, M)

# Bias forces (Coriolis, gravity)
data.qfrc_bias[:]

# Actuator forces
data.qfrc_actuator[:]
```

### MJX (JAX) Equivalents

```python
from mujoco import mjx

# Convert to MJX
mjx_model = mjx.put_model(model)
mjx_data = mjx.put_data(model, data)

# Step (returns new data, immutable)
mjx_data = mjx.step(mjx_model, mjx_data)

# Batched operations
batched_step = jax.vmap(lambda d: mjx.step(mjx_model, d))
all_data = batched_step(batched_data)

# Access data (same structure, but JAX arrays)
mjx_data.qpos  # jnp.ndarray
mjx_data.xpos  # jnp.ndarray

# Back to numpy for visualization
data = mjx.get_data(model, mjx_data)
```

---

## 7. Troubleshooting 2026

### Exploding Gradients / NaN Values

| Symptom | Cause | Solution |
|---------|-------|----------|
| NaN in loss after N steps | Gradient explosion | ✅ Reduce `max_grad_norm` to 0.1-0.5 |
| NaN in observations | Physics instability | ✅ Reduce `timestep` in MJCF, increase `n_substeps` |
| NaN in rewards | Division by zero | ✅ Add `eps=1e-8` to all divisions |
| Sudden policy collapse | Learning rate too high | ✅ Use LR warmup + cosine annealing |
| Inf values in qvel | Unconstrained actuators | ✅ Add `forcerange` limits to actuators |

```python
# Gradient clipping (mandatory for robotics)
grads = jax.grad(loss_fn)(params)
grads = jax.tree_map(lambda g: jnp.clip(g, -1.0, 1.0), grads)

# Safe reward computation
def safe_exp_reward(error, scale=1.0, eps=1e-8):
    return jnp.exp(-scale * jnp.clip(error, 0, 10))

# Observation clipping
obs = jnp.clip(obs, -obs_clip, obs_clip)
obs = jnp.nan_to_num(obs, nan=0.0, posinf=obs_clip, neginf=-obs_clip)
```

### Reward Hacking

| Symptom | Cause | Solution |
|---------|-------|----------|
| Robot vibrates in place | Energy penalty too low | ✅ Increase action smoothness penalty |
| Unnatural gaits | Velocity-only reward | ✅ Add gait phase rewards, foot clearance |
| Exploiting contact | Reward for foot contact | ✅ Use contact *schedule*, not raw contact |
| Standing still optimal | Survival bonus too high | ✅ Remove constant alive bonus, use task rewards |
| Joint limit bouncing | No limit penalty | ✅ Add soft limit penalty before hard limits |
| Falling forward for velocity | Only horizontal velocity | ✅ Add uprightness, head height rewards |

```python
# Anti-reward-hacking reward design
def robust_locomotion_reward(data, cmd_vel, prev_action, action):
    # Primary: Exponential for robustness
    vel_error = jnp.sum((cmd_vel[:2] - data.qvel[:2])**2)
    tracking = jnp.exp(-vel_error / 0.25)  # Saturates at 1.0
    
    # Secondary: Regularization
    action_rate = jnp.sum((action - prev_action)**2)
    torque = jnp.sum(jnp.abs(data.qfrc_actuator))
    
    # Hard constraints as penalties (not rewards)
    joint_limit_cost = jnp.sum(jnp.maximum(jnp.abs(data.qpos[7:]) - 0.9*joint_limits, 0)**2)
    
    return tracking - 0.01*torque - 0.1*action_rate - 10*joint_limit_cost
```

### Unstable Contact Dynamics

| Symptom | Cause | Solution |
|---------|-------|----------|
| Jittery contacts | `solref` too stiff | ✅ Increase `solref[0]` (time constant 0.02-0.05) |
| Foot sinking | `solimp` too soft | ✅ Decrease `solimp[0]` (impedance 0.9-0.99) |
| Bouncing on contact | `solref` too bouncy | ✅ Increase `solref[1]` (damping ratio 0.9-1.0) |
| Sliding feet | Friction too low | ✅ Increase `friction[0]` to 1.0-2.0 |
| Contact chattering | `impratio` too low | ✅ Increase `impratio` in `<option>` |
| Tunneling through floor | Timestep too large | ✅ Reduce timestep, increase substeps |

```xml
<option impratio="5" timestep="0.002">
  <flag warmstart="enable"/>
</option>

<default>
  <!-- Stable contact parameters -->
  <geom solref="0.02 1.0" solimp="0.9 0.95 0.001" friction="1.0 0.005 0.0001"/>
</default>
```

### Training Instability

| Symptom | Cause | Solution |
|---------|-------|----------|
| Policy doesn't improve | Bad observation normalization | ✅ Enable `normalize_obs`, clip to ±10 |
| Premature convergence | Entropy too low | ✅ Increase `ent_coef`, check log_std init |
| High variance in returns | Batch size too small | ✅ Increase `num_envs`, reduce horizon |
| KL divergence spikes | Update too aggressive | ✅ Enable `target_kl` early stopping |
| Reward scale issues | Rewards too large/small | ✅ Normalize returns, scale rewards 0.1-10 |

```python
# Observation normalization (running mean/std)
class RunningMeanStd:
    def __init__(self, shape):
        self.mean = jnp.zeros(shape)
        self.var = jnp.ones(shape)
        self.count = 1e-4
    
    def update(self, batch):
        batch_mean = jnp.mean(batch, axis=0)
        batch_var = jnp.var(batch, axis=0)
        batch_count = batch.shape[0]
        self.update_from_moments(batch_mean, batch_var, batch_count)
    
    def normalize(self, x):
        return (x - self.mean) / jnp.sqrt(self.var + 1e-8)

# Log standard deviation initialization
log_std = self.param('log_std', 
    lambda key, shape: jnp.full(shape, -0.5),  # Start with moderate exploration
    (action_dim,)
)
```

### Sim-to-Real Transfer Failures

| Symptom | Cause | Solution |
|---------|-------|----------|
| Robot falls immediately | Missing observation delay | ✅ Add 10-30ms latency in sim |
| Jerky motion on hardware | No action filtering | ✅ Low-pass filter actions, increase smoothness penalty |
| Different gait | Wrong dynamics | ✅ System ID for mass, inertia, damping |
| Motor overheating | Torque limits wrong | ✅ Match `forcerange` to hardware specs |
| Sensor drift | Missing sensor noise | ✅ Add IMU bias, noise in training |

```python
# Action filtering for sim-to-real
class ActionFilter:
    def __init__(self, alpha=0.8):
        self.alpha = alpha
        self.prev_action = None
    
    def __call__(self, action):
        if self.prev_action is None:
            self.prev_action = action
        filtered = self.alpha * action + (1 - self.alpha) * self.prev_action
        self.prev_action = filtered
        return filtered

# Latency simulation
class LatencyWrapper:
    def __init__(self, env, obs_delay=2, action_delay=1):
        self.obs_buffer = deque(maxlen=obs_delay + 1)
        self.action_buffer = deque(maxlen=action_delay + 1)
    
    def step(self, action):
        self.action_buffer.append(action)
        delayed_action = self.action_buffer[0]
        obs, reward, done, info = self.env.step(delayed_action)
        self.obs_buffer.append(obs)
        return self.obs_buffer[0], reward, done, info
```

---

## Quick Reference Card

### Common Observation Spaces

| Robot Type | Observation Components | Typical Dim |
|------------|----------------------|-------------|
| **Humanoid** | qpos(27) + qvel(27) + cinert(130) + cvel(78) + cfrc(84) | ~350 |
| **Quadruped** | qpos(19) + qvel(18) + foot_contacts(4) + cmd(3) | ~50-100 |
| **Hand** | qpos(24) + qvel(24) + fingertip_pos(15) + obj_pose(7) | ~80-120 |

### Hyperparameter Quick Guide

| Scenario | Batch Size | LR | Horizon | Entropy |
|----------|------------|-----|---------|---------|
| **Exploration phase** | 2048-4096 | 1e-3 | 32 | 0.01-0.1 |
| **Exploitation phase** | 8192+ | 1e-4 | 16 | 0.001-0.01 |
| **Fine-tuning** | 1024 | 1e-5 | 64 | 0.0001 |
| **Sim-to-Real** | 4096 | 3e-4 | 24 | 0.005 |

### Timestep Guidelines

| Robot Mass | MuJoCo Timestep | Substeps | Control Hz |
|------------|-----------------|----------|------------|
| < 5 kg | 0.002s | 4-5 | 100-200 Hz |
| 5-50 kg | 0.001-0.002s | 4-8 | 200-500 Hz |
| > 50 kg | 0.0005-0.001s | 8-20 | 500-1000 Hz |

---

*Handbook Version: 2026.1 | MuJoCo 3.x / MJX 3.x | Last Updated: January 2026*

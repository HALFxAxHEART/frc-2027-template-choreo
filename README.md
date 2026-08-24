# FRC 2027 Swerve Template — Choreo + Limelight + AdvantageKit

A **teaching template** for WPILib **2027** on the **SystemCore** (or the
Raspberry Pi "SystemCore clone"). Same as its sibling
`frc-2027-template-pathplanner`, but it uses **Choreo** for autonomous trajectory
following instead of PathPlanner.

- **Swerve drivetrain** (CTRE Phoenix 6, AdvantageKit `talonfx-swerve` base).
- **AdvantageKit logging** → view live or replay in **AdvantageScope**.
- **Limelight vision** — two cameras (a Limelight 3/2 + the USB camera on the
  SystemCore) feeding AprilTag pose estimates into the drivetrain.
- **Choreo** for smooth, time-optimal autonomous paths.
- Heavy comments throughout for students.

## ⚠️⚠️ Choreo + WPILib alpha-6 compatibility (read this)

As of now, the only 2027 ChoreoLib release (`2027.0.0-alpha-1`) was built against
an **earlier** WPILib 2027 alpha, **not** alpha-6. So this repo is **fully set up
and ready**, but it may **not compile against WPILib alpha-6 until ChoreoLib ships
an alpha-6-compatible release**. Everything else (drivetrain, vision, logging) is
independent of Choreo. If you need working autonomy *today*, use the PathPlanner
template; switch back here once ChoreoLib updates. (Send me errors and I'll help.)

## Mechanism branches (game-specific robots)

`main` = drivetrain + vision + Choreo. Game mechanisms live on their own branches:

- **`shooter`** — flywheel shooter.
- **`pick-and-place`** — elevator + gripper.

*(More archetypes — arm-on-elevator, double-jointed arm, turret, etc. — are being
added; see the repo branches.)*

---

## How Choreo autonomy works here

1. Design paths in the **Choreo GUI app** (https://choreo.autos) and save the
   `.traj` files into `src/main/deploy/choreo/`.
2. `RobotContainer` builds an `AutoFactory` and adds each trajectory to the auto
   chooser: `autoFactory.trajectoryCmd("YourTrajectoryName")`.
3. During autonomous, Choreo calls `Drive.followTrajectory(sample)` ~50×/second;
   that method steers the robot along the path (feedforward + PID correction).

In AdvantageScope, drag `Choreo/TargetPose` and `Odometry/Robot` onto the 2D
field to watch the robot track the planned path.

---

## ⚠️ Also: WPILib 2027 is alpha

Assembled from official templates but **not compiled in a 2027 environment** —
expect **one "import + build" pass in WPILib VS Code** (its importer finishes the
namespace migration). Send any errors and they can be fixed quickly.

## Quick start

1. **Copy** locally, open in **WPILib 2027 VS Code**.
2. **Let the importer run** if prompted.
3. **Install vendordeps** (*Manage Vendor Libraries → Install new (online)*):
   - AdvantageKit: `https://github.com/Mechanical-Advantage/AdvantageKit/releases/download/v27.0.0-alpha-4/AdvantageKit.json`
   - **CTRE-Phoenix (v6)** from the list.
   - Choreo: `https://choreo.autos/lib/ChoreoLib2027Alpha.json` (see the caveat above).
4. **Set your CAN bus** in `generated/TunerConstants.java` (`kCANBus`).
5. **Set your Limelight names** in `subsystems/vision/VisionConstants.java`.
6. **Simulate first** (*WPILib: Simulate Robot Code*), then connect AdvantageScope.
7. **Deploy** and enable.

## Driver controls (Xbox controller, port 0)

| Control | Action |
|---|---|
| Left stick | Drive (field-relative) |
| Right stick X | Rotate |
| Hold A | Drive while locking heading to 0° |
| Press X | Lock wheels in an "X" |
| Press B | Reset gyro "forward" |

See **STUDENT-GUIDE.md** for the concepts (command-based robots, the AdvantageKit
IO-layer pattern, REAL/SIM/REPLAY).

> The drivetrain numbers in `TunerConstants.java` are a working example from Team
> 5090's "Stingray" robot — replace them with your own, or regenerate with CTRE
> Tuner X.


---

## This branch: `shooter-turret`

Adds a **turret + flywheel shooter** (2024/2022-style auto-aim). Operator (port 1): **Right Bumper** spins the flywheel; hold **X** to auto-aim the turret using Limelight camera 0. The turret centers itself by default.

Everything from `main` (swerve + Limelight vision + autonomy + logging) is
still here; this branch just adds the mechanism above. Run it in simulation
(*WPILib: Simulate Robot Code*) and watch the `Turret/...`
values in AdvantageScope.

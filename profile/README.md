# In development....

# 🤖 Autonomous Mobile Robot (AMR) Controller – University of Moratuwa

This repository documents the design and development of a **low-cost, feature-rich AMR (Autonomous Mobile Robot) platform** intended for indoor environments such as warehouses, factories, and hospitals. The robot combines real-time sensing, robust motor control, modular construction, and remote-monitoring capabilities—engineered by undergraduates at the Department of Electronic & Telecommunication Engineering, University of Moratuwa.

<p align="center">
  <img src="images/iso.jpg"  alt="Isometric view" height="240"/>
  <img src="images/side.jpg" alt="Side view"     height="240"/>
</p>

---

## 🧪 Project Context
This project was undertaken for **EN2160 – Electronic Design Realization** as a collaborative team assignment. The aim was to replicate—then innovate on—the commercial **Omron LD-60** AMR while keeping costs low.

> ✨ **Key Objective:** Build a reliable mobile robot capable of autonomous material handling in structured indoor environments.

---

## 🧩 Key Features
- ✅ **Autonomous Navigation** with obstacle detection (LakiBeam 1 S LiDAR)  
- ✅ **Jetson Nano** on-board SLAM & object recognition  
- ✅ **Real-time Telemetry and Remote Controlling** over Wi-Fi (Mercusys MW600UH) with future fleet-management scalability  
- ✅ **Closed-loop NEMA 24 Steppers** with 10 : 1 planetary gearboxes for precise, high-torque motion  
- ✅ **USB Touch Display** providing a user-friendly on-robot interface  
- ✅ **Efficient DC-DC Power Conversion** for motor drivers and logic systems  
- ✅ **Modular Aluminium & Steel Enclosure** with an industrial aesthetic  

---

## 🧩 System Architecture

<p align="center">
  <img src="images/AMR_Hardware_Block_Diagram.jpg" alt="Hardware Block Diagram" width="720"/>
</p>

---

## 🔩 Mechanical Design

**Key parameters**

- **Dimensions** : 82.5 cm × 60 cm × 20 cm (L × W × H)  
- **Gear Ratio** : 10 : 1  
- **Wheel Diameter** : 20 cm  
- **Wheel Configuration** : 2-wheel differential drive + 4 castors  

| View | Description |
|------|-------------|
| <img src="images/side_view.png"            width="350"/> | Side view |
| <img src="images/bottom_view.png"          width="350"/> | Bottom isometric |
| <img src="images/exploded_view.png"        width="350"/> | Exploded top view |
| <img src="images/enclosure_exploded.png"   width="350"/> | Enclosure & caster alignment |
| <img src="images/frame_skeleton.png"       width="350"/> | Frame skeleton |
| <img src="images/exploded_drive_module.png" width="350"/> | Drive-module layout |
| <img src="images/wheel_placement.png"      width="350"/> | Wheel & motor configuration |
| <img src="images/block_diagram.png"        width="350"/> | Steel chassis |

> The CAD model is for **measurement verification & demonstration only**. Not direct manufacturing.

### Additional Modules

| Component | Preview |
|-----------|---------|
| **Custom Motor Coupler** | <img src="images/CouplerTop.jpg" width="180"/> <img src="images/couplerbot.jpg" width="180"/> |
| **Steel Chassis**        | <img src="images/chassy.jpg"     width="180"/> |
| **Drive Wheel**          | <img src="images/Wheel.jpg"      width="180"/> |
| **Modified Castor**      | <img src="images/ModifiedCastor.jpg" width="180"/> |

---

## 📦 Hardware Specifications

| Component | Selection | Rationale |
|-----------|-----------|-----------|
| **MCU**          | ATmega32U4 | USB, ADC, I²C, USART, rich GPIO set |
| **SBC**          | Jetson Nano | On-board SLAM, ROS2-Humble, and runnning custom SLAM implementation |
| **LiDAR**        | LakiBeam 1 S | 270 ° FOV, 18 k samples/s, Ethernet UDP |
| **Motors**       | Closed-loop NEMA 24 + 10:1 GB | High torque & encoder feedback |
| **IMU**          | Bosch BNO055 | Integrated sensor-fused drift/noise free Euler angles |
| **Communication**| Mercusys MW600UH | Dual-band, high-power Wi-Fi |
| **Display**      | 7 ″ HDMI Touch | User Interface, Local diagnostics & control |
| **Enclosure**    | Aluminium Bars and Zinc coated sheets | Strength & clean finish |

---

## 🖥️ Custom PCBs

## 🖥️ Custom PCBs

| Board | Preview&nbsp;(Top / 3D / Assembled) | Summary |
|-------|-------------------------------------|---------|
| **Microcontroller&nbsp;PCB** | <img src="images/mainTop.jpg" width="160"/> <img src="images/main3dtop.jpg" width="160"/> <img src="images/MCUSoldered.jpg" width="160"/> | **ATmega32U4**–based board that:<br>• samples IMU data, battery voltage, and cliff-sensor inputs;<br>• drives stepper motors via pulse-direction outputs;<br>• converts differential encoder signals to single-ended with on-board line receivers;<br>• exposes a USB-C port (USB-CDC) to the Jetson Nano;<br>• provides an interrupt line for the emergency-stop button. |
| **Power&nbsp;Distribution&nbsp;PCB** | <img src="images/powerTop.jpg" width="160"/> <img src="images/powerBottom.jpg" width="160"/> <img src="images/PowerSupplySoldered.jpg" width="160"/> | Steps the 22 V battery rail down to **48 V, 12 V, 5 V, and 3.3 V** using plug-in buck-converter modules and routes the main supply through the on-board power switch. |
| **Encoder&nbsp;Buffer&nbsp;PCB** | <img src="images/BufferTop.jpg" width="160"/> <img src="images/buf%203d.jpg" width="160"/> <img src="images/BufferSoldered.jpg" width="160"/> | Buffers all **eight** quadrature-encoder channels (two motors) and actively drives them so the signals can be split to both the MCU and the stepper-motor drivers without loading or skew. |
| **IMU&nbsp;Adapter&nbsp;PCB** | <img src="images/imu%20pcb.jpg" width="160"/> <img src="images/imu%203d.jpg" width="160"/> <img src="images/imu%20sol.jpg" width="160"/> | Provides modular mounting options for the IMU, to minimise magnetic interference and easy replacement; connects to the main board via a dedicated ISP adapter cable. |

---

## 📈 Performance Metrics

- **Estimated Total Weight** : **28.8 kg**  
- **Output Torque @ Wheel** : **≈ 6.0 N·m**  
- **Acceleration** : **≈ 5.88 m s⁻²**  
- **Displacement Accuracy** : **≈ 2.4 mm / step** (Changed)  

---

## 🧠 Intelligence Stack
| Layer | Description |
|-------|-------------|
| **SLAM & Obstacle Avoidance** | Real-time LiDAR processing and SLAM on Jetson Nano |
| **Low-level Motion Control** | Step-pulse generation via ATmega32U4 |
| **System Communication** | USB CDC + UART |

---

## Challenges 



---

## 🗂 Documentation

- [📘 Current Design Report (PDF)](./Design_Report.pdf)  
- [📦 Image Assets](./images)

---

## 📣 Future Enhancements

- 🔋 Battery Management System (BMS) with SoC tracking  
- 📦 Swappable payload modules  
- 🧭 Autonomous docking & charging  
- 🌐 Full ROS 2 support with fleet-level coordination  
- 📊 Cloud dashboard & OTA updates  

---

## 👥 Stakeholders & Use Cases

- 🏭 Warehouse logistics automation  
- 🏥 Hospital supply transport  
- 🛍️ Retail floor delivery  
- 🎓 Academic research labs  

---

## 🏁 Conclusion
This prototype balances **industrial performance** with **academic accessibility**, demonstrating that carefully-chosen low-cost components can rival premium AMR solutions.

> _Designed by students, built for the future of autonomous mobility._

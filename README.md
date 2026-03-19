# IoT-Based-Hybrid-Portable-Attendance-Tracking-System-With-Cloud-Integration

## Objective 
The primary objective of this project was to develop and deploy a fully functional, portable attendance tracking device capable of secure, real-time data acquisition and cloud synchronization. The implemented system successfully integrates biometric fingerprint authentication with RFID-based identification to prevent proxy attendance and ensure redundancy. A cloud-connected architecture was realized using Wi-Fi-enabled microcontroller hardware, enabling automated attendance logging, real-time dashboard visualization, and remote data access through a web interface. The system also supports attendance analysis and monitoring, fulfilling the functional and operational requirements defined at the design stag

## System Architecture

The implemented system follows a three-tier architecture to enable secure, scalable, and asynchronous data handling. The architecture was realized and deployed as part of the working system and validated during practical operation. The separation of concerns between presentation, middleware, and backend layers ensures modularity, reliability, and ease of maintenance.
1)	Presentation Layer (Frontend): This is a Single Page Application (SPA) hosted on Vercel. It is built with HTML5 and Vanilla JavaScript. It uses asynchronous fetch() requests to render the dashboard dynamically without reloading the page. This ensures a responsive user experience.

2)	Middleware Layer (Node.js Proxy): This critical component runs as a Serverless Function on Vercel. It serves as an API Gateway to resolve Cross-Origin Resource Sharing (CORS) restrictions and session conflicts that happen when browsers connect directly to Google services. It sanitizes data and adds necessary security headers before relaying information.

3)	Backend & Persistence Layer: The execution engine is Google Apps Script, which acts as a REST API endpoint that parses parameters and runs business logic. Google Sheets serves as the relational database, where rows represent attendance records.

![Slide1](https://github.com/user-attachments/assets/4b57a72a-a0f7-4d35-aaef-283624adf062)



A) Cloud Integration : The implemented attendance tracking system integrates a cloud-based infrastructure to enable real-time data storage, centralized monitoring, and remote administrative access. The system utilizes Internet of Things (IoT) technology to transmit attendance records collected through Radio Frequency Identification (RFID) and biometric authentication modules to a secure cloud server via a Wi-Fi communication interface. The implemented architecture ensures continuous synchronization of attendance data, thereby eliminating reliance on local storage and enhancing data reliability. Cloud deployment enables automated timestamp validation, secure record maintenance, remote accessibility, and scalable database management. This integration improves institutional efficiency by allowing authorized users to access attendance reports and statistical analytics from any location.

B) DevOps & CI/CD: The project uses GitHub for version control and Vercel’s Automated Pipeline for Continuous Deployment (CI/CD). Any code pushed to the repository is automatically built and deployed. This streamlines the development process.

C) Real-Time Data Visualization: The web application retrieves data stored in the cloud (Google Sheets/Database) and displays it in a responsive, user-friendly interface.

D) Security: The implemented system incorporates multiple security mechanisms to safeguard attendance records and prevent unauthorized access. Data transmission between the device and cloud server is encrypted using secure communication protocols. Device-level authentication ensures that only registered hardware units can transmit data to the cloud database. Role-based access control restricts dashboard privileges to authorized administrators. Timestamp validation mechanisms protect against replay attacks and unauthorized data manipulation. The hybrid authentication mechanism significantly reduces the likelihood of proxy attendance by requiring dual verification. Cloud-based backup and integrity verification procedures further ensure protection against data loss and tampering.

E) Email Notification: The server is set up to automatically send email alerts to parents, updating them on their ward's attendance.

## Methodology 

The system uses a strong Dual-Mode authentication process:

Primary (Biometric): The system starts by checking the R307 sensor. The sensor captures the image, processes the minutiae points, and compares them to stored templates. If a match is found, it sends the unique student ID for logging.

Secondary (RFID): If the biometric scan fails or times out, the system uses the RFID reader. It captures the unique Tag ID (UID) from the student's ID card, links it to a student ID through a database lookup, and logs the attendance.

After receiving a valid ID (from R307 or RFID), the microcontroller connects to the Wi-Fi network. It creates an HTTP GET request to a Google Apps Script web app URL. The request includes parameters such as the student ID and timestamp (e.g., ?id=12&time=2025-10-15...). The script adds this data as a new row in the Google Sheet. Successful synchronization was verified through real-time entry updates in Google Sheets, with accurate timestamping and student identification observed during system testing.

A web-based attendance dashboard was implemented and deployed to provide real-time visualization of attendance data. The system includes a secure student login interface, subject-wise attendance summary, and detailed attendance history view. Attendance percentages and lecture-wise status are dynamically fetched from the cloud database and rendered using asynchronous client-side requests. The dashboard enables users to view cumulative attendance trends and individual session records, confirming successful end-to-end data flow from the hardware device to the cloud and user interface.

The implemented hybrid IoT-based attendance tracking system was developed, assembled, and tested in a real-time academic environment to evaluate its performance, reliability, and security. The system integrates dual-mode authentication, wireless cloud synchronization, and web-based monitoring. The complete implementation is categorized into hardware system design and system interface screenshots, as described below

## Hardware Design

1)	Central Processing Unit: ESP8266 (NodeMCU)
The main controller for the system is the NodeMCU Development Board, which is based on the ESP8266 Wi-Fi chip. This unit was selected and implemented because it has an integrated TCP/IP stack, necessary for direct cloud communication without needing extra network modules.

* Functionality: The microcontroller works as the master device. It polls the peripheral sensors for data, processes the unique identifiers (student IDs), and creates the HTTP GET requests for data transmission.

* Pin Configuration: The module runs at 3.3V logic levels. It connects with the sensors using different communication protocols; SPI for the RFID reader and SoftwareSerial (UART) for the fingerprint sensor.  

<img width="1000" height="1000" alt="image" src="https://github.com/user-attachments/assets/d3a01cea-db2e-4c4e-9c88-f348f6a1eaff" />

2)	Biometric Acquisition Module: R307s Optical Sensor  
The main layer of security comes from the R307s Optical Fingerprint Sensor, which combines an optical scanner with a high-performance Digital Signal Processor (DSP).

* Working Principle: The sensor uses a dark background optical method. When a finger is placed on the collection window, an internal high-intensity LED lights up the ridges and valleys of the fingerprint. A high-resolution CMOS camera captures this illuminated image. The onboard DSP processes the image to find unique "minutiae points" (ridge bifurcations and endings) and creates a mathematical template saved in its internal flash memory [1], [9].

<img width="1000" height="1000" alt="image" src="https://github.com/user-attachments/assets/99b5caf9-5916-41ea-b030-d4176dd1dc86" />


3)	RFID Identification Module: MFRC522  
RFID As a reliable backup system, the setup includes the RFID, a compact reader/writer module for contactless communication at 13.56 MHz.

* Working Principle: The module creates a continuous electromagnetic field. When a passive RFID tag (student ID card) enters this field, the tag's internal antenna captures energy through electromagnetic induction [7]. The tag then modifies the field to send back its Unique Identifier (UID) to the reader [8].

* SPI Communication Protocol: Unlike the fingerprint sensor, the RFID reader needs fast data transfer and uses the SPI (Serial Peripheral Interface) bus. The microcontroller serves as the Master and the RFID as the Slave.

<img width="1000" height="1000" alt="image" src="https://github.com/user-attachments/assets/f048560b-a42d-4f1d-8222-231903341fa2" />

4)	Visual Feedback Interface: 16x2 Liquid Crystal Display (LCD)
To ensure clear communication with the user, the system uses a 16x2 Character LCD (Liquid Crystal Display) instead of the smaller OLED screens found in earlier versions. 

* Specification: The display has a 16-column by 2-row character grid and can show a total of 32 characters. This larger size makes it easier to read status messages like "Attendance Marked," "System Ready," or "Wi-Fi Connected." 

* Interface: Because of the limited GPIO pins on the microcontroller, the LCD connects using an I2C adapter (PCF8574). This setup allows the display to communicate with only two wires (SDA and SCL), keeping other pins available for the fingerprint and RFID sensors. 

* Backlight: It features a controllable LED backlight, which provides visibility in low-light classroom environments.

<img width="910" height="852" alt="image" src="https://github.com/user-attachments/assets/c8517c92-4a25-4c73-b516-e6a59da3a67c" />

5)	Energy Storage:3S Lithium-ion Battery pack
For operational portability, the system runs on a custom 3S (3-Series) 18650 Lithium-ion battery pack. Capacity & Voltage: This pack includes three 18650 cells connected in series, offering a nominal voltage of 11.1V, which can reach up to 12.6V when fully charged. Why 12V? This higher voltage ensures stable operation for the voltage regulators and enables the device to run for longer periods compared to a single-cell 3.7V setup.

<img width="500" height="406" alt="image" src="https://github.com/user-attachments/assets/deb7b5b4-329c-4614-8408-4c6645dc00f3" />

6)	Power Regulation: LM2596S Buck Convertor
Since the system now runs on a higher voltage 3S battery pack (approximately 12V), we need to lower the voltage to safe levels (5V or 3.3V) for the microcontroller and sensors.

* Problem: Connecting the 12V battery directly to the microcontroller would immediately damage the components. Linear regulators, like the 7805, are inefficient. They waste extra voltage as heat, which can be dangerous in a portable case. Solution: The system uses the LM2596S Step-Down (Buck) Converter. 

* Efficiency: Unlike linear regulators, the LM2596S switches power on and off thousands of times each second to reduce voltage. This method achieves up to 92% efficiency, which greatly cuts down heat production and increases battery life. Stability: It delivers a stable 5V output, no matter how the battery voltage changes, ensuring the sensors work correctly.
  
<img width="800" height="800" alt="image" src="https://github.com/user-attachments/assets/31c89674-95e9-467a-9b2a-447385577e4c" />

## Cloud-Integrated Interface Demonstration

This section presents the graphical and database-level validation of the implemented attendance tracking system. The included screenshots demonstrate real-time cloud synchronization, structured data storage, role-based authentication, and graphical attendance analysis.
 
    Student Database
<img width="507" height="242" alt="image" src="https://github.com/user-attachments/assets/5160645b-df5d-49b2-83cf-d633b4183945" />

The student data registration sheet shown in Fig. 2 illustrates the mapping between biometric identifiers, RFID credentials, and student information. This backend structure ensures accurate linking between hardware authentication and stored student records.
 
    Students Attendance in Sheets
<img width="506" height="243" alt="image" src="https://github.com/user-attachments/assets/370e8512-0c96-4939-892c-799a84f99ad4" />

The attendance computation sheet shown in Fig. 3 demonstrates automated calculation of total classes, attended classes, and attendance percentage. The structured tabular format confirms successful real-time data logging and statistical processing.
 
    Teacher Login Data
<img width="506" height="243" alt="image" src="https://github.com/user-attachments/assets/70047b2f-2beb-470f-b24a-228e56d45ec2" />

The teacher credential management sheet shown in Fig. 4 defines role-based access control for faculty members. This enables secure login and subject-wise attendance monitoring.
 
    Teacher Login Page
<img width="506" height="242" alt="image" src="https://github.com/user-attachments/assets/5d4c3653-8d83-4981-9221-a38b9c264fa1" />

The teacher login interface shown in Fig. 5 verifies authentication functionality for faculty access.
 
    Teacher Dashboard
<img width="506" height="242" alt="image" src="https://github.com/user-attachments/assets/dca704f5-76ad-4417-82ea-1540e0cb3933" />

Upon successful login, the teacher dashboard displayed in Fig. 6 provides subject-wise attendance records, percentage summaries, and daily status indicators for each student.
 
    Student Login Page
<img width="506" height="257" alt="image" src="https://github.com/user-attachments/assets/4389aa34-a157-4f1a-91a8-2560a9149aaf" />

The student login interface shown in Fig. 7 demonstrates individual-level authentication.
 
    Student Dashboard
<img width="506" height="240" alt="image" src="https://github.com/user-attachments/assets/0cede67a-6d35-4536-8965-f7569afe33fa" />

After login, the student dashboard shown in Fig. 8 presents attendance percentage summaries for each subject in a clear and structured format.
 
    Detailed Attendance View
<img width="506" height="242" alt="image" src="https://github.com/user-attachments/assets/f32285ed-7772-47bb-a8b3-401007ef089d" />

The attendance history interface shown in Fig. 9 highlights graphical visualization of cumulative attendance along with date-wise status records. This confirms that the system not only records attendance but also performs analytical representation for improved monitoring and transparency.

Together, these screenshots validate complete cloud integration, secure authentication, automated percentage calculation, and graphical attendance analysis. The results confirm that the implemented system operates reliably and provides real-time, transparent attendance monitoring for both faculty and students.

## Portability & Power Management

Power Protection: 3S Battery Management System (BMS)
To safely power the 12V system, a 3S BMS connects to the series-connected Lithium-ion cells. This module serves as the "Guardian" of the power pack. Overcharge Protection: It automatically stops charging if any individual cell voltage goes over 4.2V, which prevents swelling or explosion. Over-discharge 
Protection: It cuts the power output if the voltage falls below a critical level. This stops the battery cells from dying permanently. Cell Balancing: The BMS makes sure that all three cells charge and discharge at the same rate. This prevents one cell from wearing out faster than the others and helps maximize the overall lifespan of the portable device.

Energy Storage:3S Lithium-ion Battery pack
To ensure the system meets the "portable" requirement for a full college day, we conducted a power consumption analysis. The 3S battery pack (11.1V, 3000mAh) provides an energy capacity of about 33.3 Watt-hours (Wh).
The estimated power load of the active components, including the microcontroller (~0.40W), R307S sensor (~0.70W), LCD (~0.15W), and conversion losses, totals roughly 1.5 Watts during operation.

Using the formula Runtime = (Total Energy)/(Total Load), the theoretical runtime is 22.2 hours. However, after applying a standard safety factor of 0.7 to account for BMS cutoff voltage and real-world inefficiencies, the system has a safe usable runtime of about 15.5 hours on a single charge. This shows that the device can run for a full working day without needing to recharge.
The calculated runtime estimation was validated during continuous operation testing, confirming that the system can operate for a full academic day on a single battery charge under normal usage conditions.
I. User Controls & Enclouser: To ensure physical usability, the enclosure has two external control mechanisms:
Master Power Switch: A physical toggle switch serves as a hard cutoff between the BMS and the Buck Converter. This lets the user completely isolate the power source during storage or transport. 
System Reset Button: A momentary push-button connects the ESP8266’s RST pin to Ground (GND). Pressing this button triggers a hardware reset, forcing the microcontroller to restart its code. This offers a manual recovery option if the Wi-Fi connection hangs or the system becomes unresponsive.

## Conclusion 

This project has successfully designed and implemented a Portable Attendance Tracking System that addresses the main problems found in traditional methods. By using the R307 fingerprint sensor and RFID RFID reader, the system provides a secure, dual-authentication layer that significantly reduces the chances of proxy attendance and ensures operational backup [4], [5]. A key achievement is the creation of a fully functional, two-way data management system, where the Google Sheets backend syncs easily with the administrative website. The result is a compact, automated, and user-friendly attendance solution that is secure at the hardware level and easy to manage at the software level.

## References 

R. Memane, S. Mathapati, P. Jadhav, J. Patil, and A. Pawar, "Attendance Monitoring System Using Fingerprint Authentication," in 2022 6th International Conference On Computing, Communication, Control And Automation (ICCUBEA), Pune, India, 2022.

[2] UIDAI, "Biometric Attendance System Installation Guide," Government of India, 2018.

[3] "Fingerprint Based Students Attendance System using GSM," in International Journal of Engineering Research & Technology (IJERT) - ICONNECT Conference, 2018.

[4] L. S. Admuthe, S. J. Patil, D. D. Retharekar, A. A. Patil, A. S. Patil, and P. S. Joshi, "Fingerprint Authentication and Portable Attendance Marking Device," International Journal of Research in Engineering, Science and Management (IJRESM), vol. 3, no. 5, pp. 116-118, May 2020.

[5] "IoT-Based Attendance System using RFID and Fingerprint," Palestine Polytechnic University, 2023-24.

[6] A. Jadhav, A. Ghodse, H. Nahate, S. Ghumare, and R. Kubde, "Smart Attendance Monitoring System using Biometric," SSRN Electronic Journal, Mar. 2023.

[7] F. Ayodele, H. Singh, and E. G. AbdAllah, "Securing RFID-Based Attendance Management Systems: An Implementation of the AES Block Cipher Algorithm," in 2023 IEEE International Conference on RFID Technology and Applications (RFID-TA), Aveiro, Portugal, 2023, pp. 99-102.

[8] D. Mijić, J. Durutović, O. Bjelica, and M. Ljubojević, "An Improved Version of Student Attendance Management System Based on RFID," in 18th International Symposium INFOTEH-JAHORINA, 2019, pp. 1-5.

[9] Maurizfa and T. Adiono, "Smart Attendance Recording Device Based on Fingerprint Identification," in 2021 International Symposium on Electronics and Smart Devices (ISESD), Bandung, Indonesia, 2021.

[10] E. I. A. Ujan and I. A. Ismaili, "Biometric Attendance System," in The 2011 IEEE/ICME International Conference on Complex Medical Engineering, Harbin, China, May 2011, pp. 499-501.


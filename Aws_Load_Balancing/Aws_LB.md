Aws Load Balancing:
==================

* Aws LB will support 3 diff types of Lb.
  1. Application LB
  2. Network LB
  3. Gateway LB

* Before Talk about Lb we have to discuss two things:
  1. What is this Lb ?
  2. What is OSI model 7 layers?

1. What is Lb ?

       in short form it will distribute the traffic to all back end resources equally or based up on the configured mechanisam.

2. What is OSI models and it's 7 layers ?

       This OSI model tells about how our request travil from client to server. in b/w the client and the server we have total 7 layers.

       Diff LB act's at diff layers
       Eg:
       our Application LB act at layer 7
       Network LB act at layer 4

* Now we can see about the Layers in the OSI module.

Layer 7 – Application: "Layer 7 is the Application Layer, where application protocols such as HTTP, HTTPS, DNS and FTP enable communication between client applications and servers."
     
      Eg: When you enter a website URL in your browser, your browser uses an application-layer protocol such as HTTP or HTTPS to communicate with the web server.
  
Layer 6 – Layer 6 is responsible for how data is represented, encrypted/decrypted and formatted so that the receiving application can understand it.

<img width="1400" height="764" alt="image" src="https://github.com/user-attachments/assets/b45c315c-9657-4a90-bd79-fe4337e4365c" />

      Suppose you open:
      https://www.google.com

      What happens at Layer 6?
      
      1. Your browser and server use TLS to establish secure communication.
      2. Data is encrypted before being sent over the network.
      3. The receiving side decrypts the data.
      4. Data formats such as JSON, HTML, JPEG and UTF-8 represent information in understood formats.
<img width="678" height="182" alt="image" src="https://github.com/user-attachments/assets/24402950-ca38-46f3-bbc4-ad36cd2d63f1" />



Layer 5 – Session: Session Layer is responsible for establishing, managing and terminating communication sessions between two systems.

      Real-world example: Logging into an application
      Suppose you open your company's application: https://company.example.com
      What happens at Layer 5?
      1. Your client and server establish a communication session.
      2. The session maintains the communication context while you interact with the application.
      3. Communication continues during the session.
      4. When the session ends, it is terminated.
<img width="800" height="409" alt="image" src="https://github.com/user-attachments/assets/e6c81f35-f073-4689-b3a9-64dc203bf52a" />
<img width="708" height="247" alt="image" src="https://github.com/user-attachments/assets/318fa9b1-061e-4d08-95c9-b9e1f4b79d26" />
<img width="967" height="470" alt="image" src="https://github.com/user-attachments/assets/5511c5fc-bda7-4cec-9bdb-a3b4ba165693" />



Layer 4 – Transport: Transport Layer is responsible for end-to-end data delivery between systems using TCP or UDP.

     "Layer 4, the Transport Layer, provides end-to-end communication between systems using TCP or UDP, handling port numbers,
     segmentation, reliability, flow control and data delivery."

      Real-world example: Opening a website
      Suppose you open: https://www.google.com
      What happens at Layer 4?
      1. Your browser needs to communicate with the Google server.
      2. It uses TCP for reliable communication with HTTPS (HTTP/1.1 or HTTP/2).
      3. TCP establishes a connection using a three-way handshake.
      4. Data is divided into segments and transmitted.
      5. TCP handles acknowledgements, retransmission and correct ordering.
      6. The destination HTTPS service typically listens on port 443.
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/5a17944b-62cd-4141-9379-5bd8438009bb" />

<img width="735" height="390" alt="image" src="https://github.com/user-attachments/assets/be117fd9-0032-411b-a726-f07b5c8d41a1" />

Layer 3 – Network: Network Layer is responsible for logical IP addressing and routing packets from the source network to the destination network.
Layer 2 – Data Link: Data Link Layer is responsible for transferring data between devices within the same local network using MAC addresses and Ethernet frames. 
Layer 1 – Physical: Transmits raw bits through physical media such as cables, radio signals and fibre.





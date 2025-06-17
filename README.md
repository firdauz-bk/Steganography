# Stenograph Project
## Encode Page
![Main Page](images/main_page.PNG)

![Encode Audio](images/encode_audio.PNG)

![Encode Video](images/encode_video.PNG)

![Encode Audio after Uploading](images/encode_video2.PNG)

## Decode Page
![Decode Page](images/decode.png)

## Members 
**Team Members** <br>
Muhammad Azreen Bin Muhammad  <br>
Chng Song Heng Aloysius <br>
Muhammad Firdauz Bin Kamarulzaman <br>
Timothy Zoe Delaya <br>
Shaikh Mohamed Irfan  <br>
Nicholas Tan Qin Sheng <br>

## Introduction
Steganography is the art and science of hiding information within other non-secret data, enabling secure communication without drawing attention to the existence of the hidden message. This project explores steganography techniques across various media formats, specifically focusing on image, audio, and video files. By employing Least Significant Bit (LSB) encoding, we aim to demonstrate how secret messages can be embedded within these formats while maintaining their original quality and appearance.

The primary goals of this project are:
1. To develop robust encoding and decoding methods for text messages hidden in images, audio, and video files.
2. To analyze the effectiveness and capacity of different media formats for storing hidden data.
3. To create a user-friendly web application that allows users to easily encode and decode messages using these techniques.

Through this project, we hope to shed light on the practical applications of steganography in secure communication, digital rights management, and data protection.

## Stenography - Variety of media format
### 1. Image
#### 1.1 Text to Image
This section explains the **text-to-image encoding and decoding** functionality using LSB (Least Significant Bit) steganography. The program allows embedding and extracting secret messages within `.bmp` audio files.
   
#### 1.1.1 Encoding Process

1. **Reading the Image File**:
   - The image file is opened using the `PIL` (Python Imaging Library) module. The image is then converted into a NumPy array to access and manipulate its pixel data.

2. **Message Conversion**:
   - The secret text message is converted into binary format. Each character in the message is represented as an 8-bit binary number. For example, the letter "A" is `01000001` in binary.

3. **Appending the Message Terminator**:
   - A special terminator, `###END###`, is appended to the message. This ensures that the program can recognize where the message ends during decoding.

4. **Embedding the Message**:
   - The binary message is embedded into the least significant bit (LSB) of each pixel’s Red (R), Green (G), and Blue (B) channels. The program modifies each channel's last bit to match the binary message's corresponding bit.
   - Example: If a pixel has RGB values of `(124, 252, 180)`, and the next bit in the binary message is `1`, the Red channel is modified to `(125, 252, 180)`.

5. **Checking Capacity**:
   - The program checks whether the image has enough pixel capacity to store the entire message. If the message is too long, an error is raised, ensuring the message can be fully embedded.

6. **Writing the Encoded Image**:
   - The modified image is saved as a new file after embedding the message. This file now contains the hidden text message, but visually appears nearly identical to the original image.

#### 1.1.2 Decoding Process

1. **Reading the Encoded Image**:
   - The encoded image is opened, and the pixel data is retrieved in the same manner as during encoding. Each pixel’s Red, Green, and Blue channels are analyzed to extract the binary message stored in their least significant bits.

2. **Extracting the Binary Data**:
   - The binary data is extracted from the LSB of each pixel’s color channels. These bits are then concatenated to form the binary representation of the hidden message.

3. **Converting Binary to Text**:
   - The binary message is converted back into readable text. Each set of 8 bits corresponds to one ASCII character in the original message.

4. **Detecting the End Marker**:
   - The decoded text is checked for the `###END###` marker. When found, the program extracts and returns the hidden message, discarding any data beyond the marker.

#### 1.1.3 Notes

- **Pixel Capacity**: The number of bits that can be stored in the image is determined by the number of pixels. Each pixel can store three bits (one per color channel). If the message is too large to fit within the image, an error is raised during encoding.
- **Image Quality**: Modifying the least significant bits of pixels generally does not affect the visual quality of the image. However, larger messages may require more pixels, leading to slight visual differences.

##### 1.1.4 Key Functions
### 2. Audio
#### 2.1 Text to Audio
This section explains the **text-to-audio encoding and decoding** functionality using LSB (Least Significant Bit) steganography. The program allows embedding and extracting secret messages within `.wav` audio files.

##### 2.1.1 Encoding Process

1. **Reading the Audio File**:
   - The `.wav` file is opened using the `wave` module. The audio data is read as frames and then converted into a byte array.

2. **Message Conversion**:
   - The message is converted into binary format. Each character in the message is represented as an 8-bit binary number.

3. **Storing the Message Length**:
   - The length of the message (in characters) is converted into a 32-bit binary number and stored in the first 32 bytes of the audio file. This helps in decoding to know how many characters to retrieve.

4. **Embedding the Message**:
   - The binary message is embedded into the least significant bits of the audio file’s bytes. The number of bits (1-8) that are overwritten per byte is determined by the user input.

5. **Writing the Encoded Audio**:
   - The modified byte array is written back to a new `.wav` file, which now contains the hidden message.

##### 2.1.2 Decoding Process

1. **Reading the Encoded Audio**:
   - The encoded audio file is opened, and the byte data is read as it was during the encoding process.

2. **Retrieving the Message Length**:
   - The first 32 bits are extracted from the audio file, representing the length of the hidden message (in characters). This value is converted back into an integer.

3. **Extracting the Message**:
   - Using the retrieved message length, the corresponding bits from the audio file are read and decoded from their least significant bits back into the original message text.

##### 2.1.3 Key Functions

- `encode(byte_array, message, bits)`: Embeds the message into the byte array of the audio file using LSB encoding.
- `run_encode(audio_path, message, audio_output_path, bits)`: This function reads the audio file, encodes the message, and writes a new audio file with the hidden message.
- `run_decode(audio_output_path, bits)`: This function decodes the message from the audio file by extracting the least significant bits.

##### 2.1.4 Notes
- **Bit Usage**: The number of bits (1-8) selected affects both message capacity and audio quality. Using fewer bits ensures higher audio quality, while more bits allow longer messages.
- **Error Handling**: If the message length exceeds the audio file’s capacity, an error will be raised during decoding.
  
### 3. Video

#### 3.1 Text to Video

##### 3.1.1 Encoding Process

1. **Extracting Frames from Video**:
   - The video is read frame by frame using `cv2.VideoCapture()`, and each frame is saved as an image in a specific directory using the `extract_frames()` function.

2. **Converting Text to Binary**:
   - The secret text is read from a file, converted into its binary representation, and appended with an **EOF marker** (`$$$###$$$`) to signal the end of the message. This is done using the `generate_data()` function.

3. **Encoding Binary Data into Video Frames**:
   - The `lsb_encode()` function modifies the least significant bits (LSBs) of the red channel for each pixel in the selected frame to hide the binary message. The function allows the user to specify how many LSBs (1 to 8) to modify per pixel.

4. **Writing the Encoded Video**:
   - After encoding, the modified frames are written into a new video file using `ImageSequenceClip()`, and the original video's audio is added back using `create_video_with_audio()`.

##### 3.1.2 Decoding Process

1. **Extracting Frames from the Encoded Video**:
   - The encoded video is read frame by frame, and the frames are saved as images using the `extract_frames()` function.

2. **Extracting Binary Data from Frames**:
   - The `lsb_decode()` function retrieves the LSBs from the red channel of each pixel in the selected frame. The extracted binary data is processed to look for the **EOF marker** (`$$$###$$$`) to determine when the hidden message ends.

3. **Converting Binary Data to Text**:
   - Once the EOF marker is detected, the remaining binary data is split into 8-bit chunks and converted back into readable text using the `lsb_decode()` function.

##### 3.1.3 Notes

- **Video Length**: Ensure that the video has enough frames to hide the entire message. If the message is too long, encoding will fail.
- **EOF Marker**: The EOF marker (`$$$###$$$`) ensures that the hidden message is correctly detected and extracted during the decoding process.
- **LSB Bits**: The number of LSB bits to modify can be adjusted to control how much data can be encoded into each pixel.

  
## Setting up
### Pre-requisite
Please install the required Python libraries
> pip install -r requirements.txt

### Launch web application
> python app.py

## Credits & References
UX/UI - [TailwindCSS](https://tailwindcss.com/)  <br>
FFMPEG - [FFMPEG](https://ffmpeg.org/download.html#build-windows)  <br>

# ZeroLinq
Transfer text, audio, and files using sound alone — a cross-platform acoustic communication protocol.

## Overview

ZeroLinq encodes data into audio signals using digital modulation techniques (such as FSK or chirp-based encoding).  
It is designed for offline communication between devices using speakers and microphones only.

## Features

- Audio-based data transmission using audible or ultrasonic frequencies
- Automatic listening and real-time decoding
- Support for text, audio clips, and arbitrary binary files
- Packet-based framing with headers, payloads, and integrity checks
- Error detection and correction (e.g., CRC and optional ECC)
- Lightweight protocol design for mobile, embedded, and desktop systems
- Reference implementations in Swift (iOS), Python, and JavaScript (planned)

## Protocol Basics

ZeroLinq transmits data in self-contained packets:

[ SYNC ][ HEADER ][ PAYLOAD ][ CRC ]


- **SYNC**: Audio preamble to initiate detection and synchronization
- **HEADER**: Metadata (data type, payload size, flags)
- **PAYLOAD**: Encoded data (text, audio, binary chunks)
- **CRC**: Error-checking checksum (optionally enhanced with Reed-Solomon ECC)

## Use Cases

- Offline peer-to-peer data transfer between mobile devices
- Configuring IoT devices without network access
- Secure or air-gapped data delivery via sound
- Educational or research applications in acoustic communication

## Getting Started

This repository includes:

- `/protocol/`: Formal documentation for the ZeroLinq protocol
- `/reference/`: Platform-specific implementations
    - `/ios/`: Swift (iOS app)
    - `/python/`: CLI and desktop demo
    - `/js/`: Planned JavaScript decoder

This project is open-source under the MIT License.

Credits

ZeroLinq was created by Dylan Manley (Ƶ3rθDaλ).
For contributions, ideas, or integration questions, feel free to open an issue or submit a pull request.

# ZeroLinq Protocol Specification

**ZeroLinq** is an audio-based data transfer protocol for text, audio, and binary files.  
It operates over air using speakers and microphones, with no network or physical connection required.

---

## 1. Goals

- Enable reliable peer-to-peer communication using sound
- Support arbitrary payloads (text, binary, audio) via self-contained packets
- Operate across a range of environments, including noisy ones
- Be cross-platform, low-overhead, and embeddable
- Allow automatic listening and syncing without user coordination

---

## 2. Signal Encoding

ZeroLinq supports multiple encoding modes (based on environment and bandwidth):

### 2.1 Frequency-Shift Keying (FSK)

- Each bit or group of bits is mapped to a discrete frequency
- Tuned for compatibility with common microphones/speakers
- Default: 4-bit symbols (16 tones) in 2–10 kHz range

### 2.2 Chirp Spread Spectrum (optional)

- Uses frequency sweeps for better synchronization and error tolerance
- Suitable for noisy or long-range environments

---

## 3. Packet Format

Every message is packetized with the following structure:

[ SYNC ][ HEADER ][ PAYLOAD ][ CRC ]


### 3.1 SYNC

- Fixed frequency or chirp sequence to identify the start of transmission
- Helps align decoder timing
- Can also embed version info or negotiation flags

### 3.2 HEADER (8–16 bytes)

- Contains:
  - Protocol version
  - Payload type (text, file, audio)
  - Payload length
  - Flags (encryption, compression, etc.)

### 3.3 PAYLOAD

- Encoded as a stream of base symbols (FSK tone values)
- Can be chunked and streamed if large

### 3.4 CRC / ECC

- 16-bit CRC for error detection
- Optionally followed by Reed-Solomon or Hamming codes for error correction

---

## 4. Transmission Flow

1. Device A encodes data and emits audio
2. Device B continuously listens
3. On SYNC detection, decoding begins
4. If valid CRC is received, payload is passed to the application layer

---

## 5. Supported Payload Types

| Type   | ID  | Notes                        |
|--------|-----|------------------------------|
| Text   | 0x01| UTF-8 string                 |
| Audio  | 0x02| Raw or compressed clip       |
| Binary | 0x03| Arbitrary file data          |

---

## 6. Error Handling

- If CRC fails, the packet is discarded or retried
- ECC blocks may allow forward correction
- Retransmission may be supported in interactive modes

---

## 7. Automatic Listening

- Receivers keep a rolling audio buffer
- Decode attempts triggered on SYNC detection
- Fallback: periodic active sampling if passive detection fails

---

## 8. Versioning

- Protocol versions encoded in HEADER
- Backward-compatible changes allowed via flags
- Major changes require updated decoder modules

---

## 9. Security (Planned)

- Optional symmetric encryption (e.g. AES-128)
- Optional public key for handshake + payload encryption
- Keys negotiated out-of-band or via secondary channel

---

## 10. Limitations

- Subject to environmental noise and interference
- Limited bitrate (~100–1000 bps depending on mode)
- Latency may vary depending on buffer size and packet size

---

## 11. Implementations

- `iOS/`: Swift with AVFoundation
- `python/`: NumPy, SciPy-based encoder/decoder
- `js/`: Planned for browser/Node.js

---

## 12. License

ZeroLinq is open-source and MIT licensed.  
Created by Dylan Manley (Ƶ3rθDaλ).

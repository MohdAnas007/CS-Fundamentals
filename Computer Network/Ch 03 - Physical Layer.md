# Chapter 3: Physical Layer

## Introduction

The **Physical Layer** is the lowest layer in the OSI (Open Systems Interconnection) model and the TCP/IP protocol suite. It is responsible for the actual transmission of raw bits over a physical medium. This layer defines the mechanical, electrical, procedural, and functional specifications for activating, maintaining, and deactivating physical links between network devices.

**Learning Objectives:**
- Understand various transmission media and their characteristics
- Differentiate between analog and digital signals
- Comprehend different transmission modes
- Master line coding techniques used in digital communication
- Apply Nyquist and Shannon theorems to calculate channel capacity

---

## 1. Transmission Media

Transmission media serves as the physical pathway for data transmission between sender and receiver. It can be broadly classified into **guided** (wired) and **unguided** (wireless) media.

### 1.1 Guided Media (Wired)

Guided media provides a physical conduit that directs signals along a specific path. The signal is confined within the medium, offering better security and reliability compared to wireless transmission.

```mermaid
flowchart TD
    A[Guided Media] --> B[Twisted Pair]
    A --> C[Coaxial Cable]
    A --> D[Optical Fiber]

    B --> B1[UTP - Unshielded Twisted Pair]
    B --> B2[STP - Shielded Twisted Pair]
    B1 -.-> B1a[Cat5e, Cat6, Cat6a, Cat7]
    B2 -.-> B2a[STP, Screened STP]

    C --> C1[Baseband Coaxial]
    C --> C2[Broadband Coaxial]
    C1 -.-> C1a[Thinnet, Thicknet]
    C2 -.-> C2a[Cable TV, HFC Networks]

    D --> D1[Single-mode Fiber - SMF]
    D --> D2[Multi-mode Fiber - MMF]
    D1 -.-> D1a[Long distance, Telecom]
    D2 -.-> D2a[Short distance, LAN]
```

#### 1.1.1 Twisted Pair Cable

Twisted pair consists of two insulated copper wires twisted together in a helical pattern. The twisting helps reduce electromagnetic interference (EMI) and crosstalk.

| Type | Shielding | Max Bandwidth | Typical Use Case |
|------|-----------|---------------|------------------|
| **Cat5e** | None | 100 MHz | Fast Ethernet (100 Mbps) |
| **Cat6** | None/Slight | 250 MHz | Gigabit Ethernet (1 Gbps) |
| **Cat6a** | Enhanced | 500 MHz | 10 Gigabit Ethernet |
| **Cat7** | Full Shielded | 600 MHz | Data Centers |

**Advantages:**
- Low cost and easy installation
- Flexible and lightweight
- Widely available and supported

**Disadvantages:**
- Limited bandwidth compared to fiber
- Susceptible to EMI and crosstalk
- Attenuation increases with distance

#### 1.1.2 Coaxial Cable

Coaxial cable consists of a central conductor surrounded by insulation, a metallic shield, and an outer jacket. It offers better shielding than twisted pair.

```mermaid
flowchart LR
    subgraph Coaxial Structure
        A[Inner Conductor] --> B[Dielectric Insulator]
        B --> C[Metallic Shield]
        C --> D[Outer Jacket]
    end
```

| Type | Signal Type | Application | Characteristics |
|------|-------------|-------------|-----------------|
| **Baseband** | Digital | Ethernet (10Base2, 10Base5) | Single channel, digital transmission |
| **Broadband** | Analog | Cable TV, Internet | Multiple channels, analog transmission |

**Advantages:**
- Higher bandwidth than twisted pair
- Better immunity to noise
- Longer transmission distances

**Disadvantages:**
- More expensive than twisted pair
- Bulky and less flexible
- Being replaced by fiber in many applications

#### 1.1.3 Optical Fiber

Optical fiber uses light pulses to transmit data through a glass or plastic core, offering the highest bandwidth and longest transmission distances.

```mermaid
flowchart TD
    subgraph Fiber Optic Cable Structure
        A[Core - Light carrying] --> B[Cladding - Reflects light]
        B --> C[Buffer Coating - Protection]
        C --> D[Outer Jacket]
    end
```

| Type | Core Diameter | Light Source | Distance | Application |
|------|---------------|--------------|----------|-------------|
| **Single-mode (SMF)** | 8-10 μm | Laser | Up to 100 km | WAN, Telecom |
| **Multi-mode (MMF)** | 50-62.5 μm | LED/VCSEL | Up to 2 km | LAN, Data Centers |

**Advantages:**
- Extremely high bandwidth (Tbps range)
- Very low attenuation
- Immune to electromagnetic interference
- Highly secure (difficult to tap)

**Disadvantages:**
- High installation cost
- Fragile and requires careful handling
- Complex splicing and termination

---

### 1.2 Unguided Media (Wireless)

Unguided media transmits electromagnetic waves through air, vacuum, or water without requiring a physical conduit.

```mermaid
flowchart LR
    subgraph Wireless Transmission
        direction TB
        R[Radio Waves<br/>3 kHz – 1 GHz<br/>Omnidirectional<br/>Penetrates walls]
        M[Microwaves<br/>1 GHz – 300 GHz<br/>Directional<br/>Line-of-sight required]
        I[Infrared<br/>300 GHz – 400 THz<br/>Short range<br/>Cannot penetrate walls]
    end
    R --> R1[AM/FM Radio, TV, Mobile]
    M --> M1[Satellite, Point-to-Point Links]
    I --> I1[Remote Controls, IrDA]
```

#### Comparison of Wireless Media

| Feature | Radio Waves | Microwaves | Infrared |
|---------|-------------|------------|----------|
| **Frequency** | 3 kHz – 1 GHz | 1 GHz – 300 GHz | 300 GHz – 400 THz |
| **Propagation** | Omnidirectional | Unidirectional | Unidirectional |
| **Penetration** | High | Low | None |
| **Range** | Long | Medium-Long | Short |
| **Interference** | High | Medium | Low |
| **Security** | Low | Medium | High |
| **Cost** | Low | Medium | Low |

**Radio Waves:**
- Used for broadcasting (AM/FM radio, television)
- Can travel long distances and penetrate buildings
- Prone to interference from other radio sources

**Microwaves:**
- Require line-of-sight between transmitter and receiver
- Used in satellite communication and point-to-point links
- Affected by rain, fog, and atmospheric conditions

**Infrared:**
- Cannot penetrate solid objects
- Used in short-range communication (remote controls, IrDA)
- High security due to limited range

---

## 2. Signal Types: Analog vs Digital

Understanding the difference between analog and digital signals is fundamental to comprehending data transmission.

### 2.1 Analog Signals

An **analog signal** is a continuous waveform that varies smoothly over time. It can take an infinite number of values within a given range.

```mermaid
flowchart LR
    subgraph Analog Signal Characteristics
        A[Continuous] --> B[Infinite values]
        B --> C[Smooth variations]
        C --> D[Susceptible to noise]
    end
```

**Characteristics:**
- Continuous in both time and amplitude
- Represents physical quantities (sound, temperature, light)
- More susceptible to noise and distortion
- Requires amplification (which also amplifies noise)

**Examples:**
- Human voice
- Analog radio/TV signals
- Temperature sensors
- Traditional telephone systems

### 2.2 Digital Signals

A **digital signal** is a discrete signal that represents data using specific values, typically binary (0 and 1). It changes in steps rather than continuously.

```mermaid
stateDiagram-v2
    [*] --> High : Bit 1 (+V)
    High --> Low : Bit 0 (-V)
    Low --> High : Bit 1 (+V)
    High --> High2 : Bit 1 (+V)
    High2 --> Low2 : Bit 0 (-V)
    Low2 --> Low3 : Bit 0 (-V)
    Low3 --> High3 : Bit 1 (+V)
    High3 --> [*]
    
    note right of High : Voltage = +V
    note right of Low : Voltage = -V
```

**Characteristics:**
- Discrete in time and amplitude
- Uses binary representation (0s and 1s)
- Less affected by noise
- Easier to store, process, and transmit
- Can be regenerated without cumulative distortion

**Advantages of Digital over Analog:**

| Aspect | Analog | Digital |
|--------|--------|---------|
| **Noise Immunity** | Low | High |
| **Error Correction** | Difficult | Easy |
| **Storage** | Complex | Simple |
| **Processing** | Limited | Extensive |
| **Security** | Hard to encrypt | Easy to encrypt |
| **Quality over distance** | Degrades | Maintained |

### 2.3 Signal Conversion

```mermaid
flowchart LR
    subgraph Conversion Process
        A[Analog Signal] -->|Sampling| B[Discrete Samples]
        B -->|Quantization| C[Quantized Values]
        C -->|Encoding| D[Digital Signal]
    end
    
    subgraph Reverse Process
        E[Digital Signal] -->|Decoding| F[Quantized Values]
        F -->|Reconstruction| G[Analog Signal]
    end
```

**Key Conversion Techniques:**
- **PCM (Pulse Code Modulation):** Standard method for digitizing analog signals
- **Delta Modulation:** Encodes changes in signal rather than absolute values
- **ADC/DAC:** Analog-to-Digital and Digital-to-Analog Converters

---

## 3. Transmission Modes

Transmission mode defines the direction of data flow between two communicating devices.

```mermaid
sequenceDiagram
    participant A as Device A
    participant B as Device B

    Note over A,B: Simplex Mode (Unidirectional)
    A->>B: Data Transmission
    Note right of B: B cannot send data back

    Note over A,B: Half-Duplex Mode (Bidirectional, Alternating)
    A->>B: Data Transmission
    B-->>A: Acknowledgment (after A finishes)
    B->>A: Data Transmission
    A-->>B: Acknowledgment (after B finishes)

    Note over A,B: Full-Duplex Mode (Bidirectional, Simultaneous)
    par Simultaneous Transmission
        A->>B: Data
    and
        B->>A: Data
    end
```

### 3.1 Detailed Comparison

| Mode | Direction | Simultaneous | Channels | Efficiency | Example |
|------|-----------|--------------|----------|------------|---------|
| **Simplex** | One-way | N/A | 1 | Low | Keyboard, TV broadcast |
| **Half-Duplex** | Two-way | No | 1 | Medium | Walkie-talkie, CB radio |
| **Full-Duplex** | Two-way | Yes | 2 | High | Telephone, Ethernet |

### 3.2 Use Cases

**Simplex:**
- Broadcasting (radio, TV)
- Keyboard to computer
- Sensors to controllers
- Fire alarms

**Half-Duplex:**
- Walkie-talkies
- Citizen Band (CB) radio
- Early Ethernet (10Base5)
- Wireless LANs (older standards)

**Full-Duplex:**
- Telephone networks
- Modern Ethernet (switched)
- Cellular networks
- Instant messaging

---

## 4. Line Coding Techniques

Line coding is the process of converting digital data into digital signals for transmission. Different techniques offer various trade-offs between bandwidth efficiency, synchronization, and DC balance.

### 4.1 Key Concepts

```mermaid
flowchart TD
    subgraph Line Coding Objectives
        A[Line Coding] --> B[DC Balance]
        A --> C[Self-Synchronization]
        A --> D[Bandwidth Efficiency]
        A --> E[Error Detection]
        A --> F[Noise Immunity]
    end
    
    B --> B1[Prevent DC drift]
    C --> C1[Clock recovery]
    D --> D1[Maximize data rate]
    E --> E1[Detect transmission errors]
    F --> F1[Minimize noise effects]
```

**Important Terms:**
- **Baseline:** The average voltage level used as reference
- **DC Component:** Constant voltage offset that can cause problems
- **Baud Rate:** Number of signal changes per second
- **Bit Rate:** Number of bits transmitted per second

### 4.2 NRZ-L (Non-Return to Zero, Level)

In NRZ-L encoding, the voltage level directly represents the bit value.

**Encoding Rules:**
- Bit `1` → Positive voltage (+V)
- Bit `0` → Negative voltage (-V)
- No return to zero between bits

**Example:** Bits `1 0 1 1 0 0 1`

```mermaid
stateDiagram-v2
    [*] --> Vplus1 : 1 (+V)
    Vplus1 --> Vminus1 : 0 (-V)
    Vminus1 --> Vplus2 : 1 (+V)
    Vplus2 --> Vplus3 : 1 (+V)
    Vplus3 --> Vminus2 : 0 (-V)
    Vminus2 --> Vminus3 : 0 (-V)
    Vminus3 --> Vplus4 : 1 (+V)
    Vplus4 --> [*]
```

**Waveform Representation:**

| Bit | 1 | 0 | 1 | 1 | 0 | 0 | 1 |
|-----|---|---|---|---|---|---|---|
| Voltage | +V | -V | +V | +V | -V | -V | +V |

**Advantages:**
- Simple to implement
- Efficient bandwidth usage
- Good for short-distance communication

**Disadvantages:**
- DC component present (problematic for transformers)
- No self-synchronization (long runs of same bit cause loss of clock)
- No error detection capability

### 4.3 NRZ-I (Non-Return to Zero, Inverted)

In NRZ-I, the signal inverts when a `1` is encountered and stays the same for `0`.

**Encoding Rules:**
- Bit `1` → Invert the signal
- Bit `0` → No change (maintain previous level)

**Example:** Bits `1 0 1 1 0 0 1`

```mermaid
stateDiagram-v2
    [*] --> High1 : Start High
    High1 --> Low1 : 1 (Invert to Low)
    Low1 --> Low2 : 0 (No change)
    Low2 --> High2 : 1 (Invert to High)
    High2 --> Low3 : 1 (Invert to Low)
    Low3 --> Low4 : 0 (No change)
    Low4 --> Low5 : 0 (No change)
    Low5 --> High3 : 1 (Invert to High)
    High3 --> [*]
```

**Comparison: NRZ-L vs NRZ-I**

| Feature | NRZ-L | NRZ-I |
|---------|-------|-------|
| **Encoding** | Voltage = bit value | Invert on 1 |
| **DC Component** | May have | Reduced |
| **Synchronization** | Poor | Better for 1s |
| **Complexity** | Simple | Slightly complex |

### 4.4 Manchester Coding (IEEE 802.3)

Manchester encoding provides self-synchronization by ensuring a transition in the middle of every bit period.

**Encoding Rules:**
- Bit `1` → High-to-Low transition (falling edge)
- Bit `0` → Low-to-High transition (rising edge)

**Example:** Bits `1 0 1 0`

```mermaid
stateDiagram-v2
    [*] --> High1 : Bit 1 - Start High
    High1 --> Low1 : Mid-bit transition (1→0)
    Low1 --> Low2 : Bit 0 - Start Low
    Low2 --> High2 : Mid-bit transition (0→1)
    High2 --> High3 : Bit 1 - Start High
    High3 --> Low3 : Mid-bit transition (1→0)
    Low3 --> Low4 : Bit 0 - Start Low
    Low4 --> High4 : Mid-bit transition (0→1)
    High4 --> [*]
```

**Detailed Transition Table:**

| Bit | Start Level | Mid-Bit Transition | End Level | Next Start |
|-----|-------------|-------------------|-----------|------------|
| 1 | High | High → Low | Low | Low |
| 0 | Low | Low → High | High | High |
| 1 | High | High → Low | Low | Low |
| 0 | Low | Low → High | High | High |

**Differential Manchester:**
- Transition in middle of bit (for clocking)
- Transition at start of bit indicates `0`
- No transition at start indicates `1`

**Advantages:**
- Self-synchronizing (clock embedded in signal)
- No DC component
- Error detection capability

**Disadvantages:**
- Requires 2× bandwidth of NRZ
- More complex encoding/decoding

### 4.5 Bipolar AMI (Alternate Mark Inversion)

AMI uses three voltage levels: positive, zero, and negative.

**Encoding Rules:**
- Bit `0` → 0V (zero level)
- Bit `1` → Alternating +V and -V

**Example:** Bits `1 0 1 1 0 1`

```mermaid
stateDiagram-v2
    [*] --> Plus1 : 1 (+V)
    Plus1 --> Zero1 : 0 (0V)
    Zero1 --> Minus1 : 1 (-V)
    Minus1 --> Plus2 : 1 (+V)
    Plus2 --> Zero2 : 0 (0V)
    Zero2 --> Minus2 : 1 (-V)
    Minus2 --> [*]
```

**Waveform Representation:**

| Bit | 1 | 0 | 1 | 1 | 0 | 1 |
|-----|---|---|---|---|---|---|
| Voltage | +V | 0V | -V | +V | 0V | -V |

**Advantages:**
- No DC component
- Self-synchronizing for 1s
- Error detection (violation of alternation rule)
- Efficient bandwidth usage

**Disadvantages:**
- Long sequences of 0s cause loss of synchronization
- Requires three voltage levels

### 4.6 Comparison of Line Coding Techniques

| Technique | DC Component | Synchronization | Bandwidth | Error Detection | Complexity |
|-----------|--------------|-----------------|-----------|-----------------|------------|
| **NRZ-L** | Yes | Poor | 1× | No | Low |
| **NRZ-I** | Reduced | Moderate | 1× | No | Low |
| **Manchester** | No | Excellent | 2× | Yes | Medium |
| **Diff. Manchester** | No | Excellent | 2× | Yes | Medium |
| **AMI** | No | Good | 1× | Yes | Medium |

---

## 5. Data Rate Concepts

### 5.1 Nyquist Theorem (Noiseless Channel)

The Nyquist theorem defines the maximum data rate for a noiseless channel.

**Formula:**
```
C = 2 × B × log₂(L)
```

Where:
- **C** = Channel capacity (bits per second)
- **B** = Bandwidth (Hz)
- **L** = Number of signal levels

**Key Insights:**
- Maximum symbol rate = 2B symbols/second
- More levels = more bits per symbol
- Trade-off: More levels → more susceptible to noise

```mermaid
flowchart TD
    subgraph Nyquist_Theorem
        N1[Bandwidth B Hz] --> N2[Max Symbol Rate = 2B]
        N3[Levels L] --> N4[Bits per Symbol = log₂L]
        N2 --> N5[Capacity = 2B × log₂L]
        N4 --> N5
    end
```

**Example Calculation:**
- Bandwidth = 3000 Hz
- Levels = 4 (2 bits per symbol)
- C = 2 × 3000 × log₂(4) = 2 × 3000 × 2 = 12,000 bps

### 5.2 Shannon Capacity (Noisy Channel)

The Shannon theorem defines the maximum data rate for a noisy channel.

**Formula:**
```
C = B × log₂(1 + S/N)
```

Where:
- **C** = Channel capacity (bits per second)
- **B** = Bandwidth (Hz)
- **S/N** = Signal-to-Noise Ratio (linear, not dB)

**Converting SNR from dB:**
```
S/N (linear) = 10^(SNR_dB / 10)
```

```mermaid
flowchart TD
    subgraph Shannon_Capacity
        S1[Bandwidth B Hz] --> S3[Capacity Formula]
        S2[SNR S/N] --> S3
        S3 --> S4[C = B × log₂ 1 + S/N]
    end
    
    subgraph Practical_Limit
        P1[Actual Rate ≤ Shannon Capacity]
        P2[Noise limits bits per symbol]
        P1 --> P3[Channel is fundamentally limited]
        P2 --> P3
    end
```

**Key Insights:**
- Shannon capacity is the theoretical maximum
- Cannot be exceeded regardless of encoding
- Noise fundamentally limits channel capacity
- Higher SNR allows more signal levels

### 5.3 Relationship Between Nyquist and Shannon

```mermaid
flowchart TD
    subgraph Nyquist_Analysis
        N1["Max Symbol Rate = 2B"]
        N2["With L levels: C = 2B log₂L"]
    end
    
    subgraph Shannon_Analysis
        S1["SNR determines max levels"]
        S2["L_max = √ 1 + S/N"]
        S3["C = B log₂ 1 + S/N"]
    end
    
    N2 --> R[Practical Data Rate]
    S3 --> R
    
    R --> C["Actual Rate ≤ min Nyquist, Shannon"]
    
    style R fill:#f9f,stroke:#333
    style C fill:#bbf,stroke:#333
```

**Unified View:**
1. Nyquist tells us how fast we can send symbols (no noise consideration)
2. Shannon tells us how many bits per symbol are reliable (noise consideration)
3. Combining both: Maximum reliable data rate

### 5.4 Comprehensive Example

**Problem:** A telephone line has:
- Bandwidth (B) = 3.1 kHz = 3100 Hz
- SNR = 30 dB

**Solution:**

**Step 1: Convert SNR to linear scale**
```
S/N = 10^(30/10) = 10^3 = 1000
```

**Step 2: Calculate Shannon Capacity**
```
C = B × log₂(1 + S/N)
C = 3100 × log₂(1001)
C = 3100 × 9.97
C ≈ 30,900 bps ≈ 30.9 kbps
```

**Step 3: Determine maximum levels using Shannon**
```
L_max = √(1 + S/N) = √1001 ≈ 31.6 ≈ 31 levels
```

**Step 4: Calculate Nyquist Capacity with L = 31**
```
C = 2B × log₂(L)
C = 2 × 3100 × log₂(31)
C = 6200 × 4.95
C ≈ 30,690 bps ≈ 30.7 kbps
```

**Step 5: Compare with L = 4 (if only 4 levels used)**
```
C = 2 × 3100 × log₂(4) = 2 × 3100 × 2 = 12,400 bps
```

**Conclusion:** 
- With only 4 levels, the channel is Nyquist-limited (12.4 kbps)
- With optimal levels (31), we approach Shannon capacity (30.9 kbps)
- The channel is fundamentally limited by noise (Shannon)

### 5.5 Practical Implications

| Scenario | Limiting Factor | Solution |
|----------|-----------------|----------|
| Low noise, limited bandwidth | Nyquist | Increase signal levels |
| High noise | Shannon | Improve SNR or reduce rate |
| Both limited | Shannon | Fundamental channel limit |
| Long distance | Attenuation | Amplifiers/Regenerators |

---

## Summary Tables

### Transmission Media Comparison

| Medium | Bandwidth | Distance | Cost | EMI Immunity | Security |
|--------|-----------|----------|------|--------------|----------|
| **Twisted Pair** | Low-Medium | Short | Low | Low | Low |
| **Coaxial** | Medium | Medium | Medium | Medium | Medium |
| **Fiber Optic** | Very High | Very Long | High | Excellent | Excellent |
| **Radio** | Low-Medium | Long | Low | N/A | Low |
| **Microwave** | High | Long | Medium | N/A | Medium |
| **Infrared** | High | Short | Low | N/A | High |

### Line Coding Summary

| Technique | DC Component | Sync | Bandwidth | Error Detection |
|-----------|--------------|------|-----------|-----------------|
| **NRZ-L** | Yes | Poor | 1× | No |
| **NRZ-I** | Reduced | Fair | 1× | No |
| **Manchester** | No | Excellent | 2× | Yes |
| **Diff. Manchester** | No | Excellent | 2× | Yes |
| **AMI** | No | Good | 1× | Yes |

### Capacity Formulas

| Theorem | Formula | Application | Limiting Factor |
|---------|---------|-------------|-----------------|
| **Nyquist** | C = 2B log₂(L) | Noiseless channel | Bandwidth & levels |
| **Shannon** | C = B log₂(1 + S/N) | Noisy channel | Bandwidth & SNR |

---

## Key Takeaways

1. **Transmission media** choice depends on bandwidth requirements, distance, cost, and environmental factors

2. **Digital signals** are preferred over analog for their noise immunity, ease of processing, and error correction capabilities

3. **Transmission modes** (simplex, half-duplex, full-duplex) determine the direction and simultaneity of data flow

4. **Line coding** techniques balance trade-offs between DC balance, synchronization, bandwidth efficiency, and error detection

5. **Nyquist theorem** defines the maximum symbol rate for noiseless channels

6. **Shannon theorem** defines the fundamental capacity limit imposed by noise

7. **Practical data rate** is always ≤ min(Nyquist capacity, Shannon capacity)

---


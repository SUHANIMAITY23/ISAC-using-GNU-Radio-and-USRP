# Integrated Sensing and Communication (ISAC) Using GNU Radio and USRP

A software-defined radio (SDR) prototype demonstrating how a wireless
signal can support **communication and environmental sensing at the same
time**.

This project was completed as a summer internship project at
**Visvesvaraya National Institute of Technology (VNIT)** in 2025.

## Overview

Integrated Sensing and Communication (ISAC) uses a shared radio waveform
for both transmitting information and sensing the surrounding
environment. In this project, two GNU Radio flowgraphs were developed
and tested using USRP hardware:

1.  **BPSK bit communication:** User-defined bits are transmitted using
    manually implemented Binary Phase Shift Keying (BPSK), while
    received signal power is monitored for sensing.
2.  **WBFM audio communication:** Audio from a WAV file is transmitted
    using Wideband Frequency Modulation (WBFM). A low-amplitude 1 kHz
    cosine signal is added as a sensing component, and its received
    strength is monitored.

The experiments investigate changes in received power when objects are
placed near the antenna or moved within the sensing region.

## Project Objectives

-   Build an ISAC demonstration using GNU Radio and USRP.
-   Implement a manual BPSK transmitter and receiver for bit
    communication.
-   Implement WBFM-based audio transmission and reception.
-   Integrate sensing by measuring received signal power / received
    signal strength (RSS).
-   Observe changes associated with object presence, movement, and metal
    versus non-metal objects.

## Tools and Hardware

  -----------------------------------------------------------------------
  Component                           Purpose
  ----------------------------------- -----------------------------------
  GNU Radio                           Design and execute the
                                      communication and sensing
                                      flowgraphs

  Python blocks                       Perform power averaging

  USRP                                RF signal transmission and
                                      reception

  Antennas                            Omnidirectional and rectangular
                                      wideband antennas
  -----------------------------------------------------------------------

### Main Parameters

  Parameter                Configuration
  ------------------------ -----------------------------------------
  RF centre frequency      3.5 GHz
  Sampling rate -- BPSK    64 kS/s
  Sampling rate -- audio   48 kS/s
  Gain                     Configured according to the environment

## Implementation

### 1. Manual BPSK Communication with Sensing

**Transmitter**

1.  A Vector Source generates a user-defined bit sequence.
2.  Bits are mapped to BPSK symbols: `0 → -1` and `1 → +1`.
3.  The symbols are multiplied by a cosine carrier to generate the
    modulated signal.
4.  The signal is transmitted through the USRP Sink.

**Receiver**

1.  The USRP Source captures the received waveform.
2.  AGC2 and AGC3 compensate for amplitude variations.
3.  A Costas Loop recovers the carrier phase.
4.  Moving-average and low-pass filtering smooth the signal and reduce
    high-frequency noise.
5.  A Binary Slicer recovers the bits: positive values map to `1`, and
    negative values map to `0`.
6.  The recovered sequence is compared with the transmitted sequence.

**Sensing**

The received signal is squared and averaged to estimate its power. A
moving-average filter smooths the power measurements. Changes in the
resulting signal-strength estimate are observed while objects are
introduced or moved near the antenna.

### 2. WBFM Audio Communication with Sensing

**Transmitter**

1.  Audio samples are read from a WAV file.
2.  A WBFM Transmit block modulates the audio.
3.  A Rational Resampler adjusts the sample rate for the USRP.
4.  A 1 kHz cosine wave with amplitude `0.1` is added to the resampled
    audio as a sensing component.
5.  The combined signal is transmitted through the USRP Sink.

**Receiver**

1.  The USRP Source captures the transmitted signal.
2.  The signal is resampled as required and passed through a WBFM
    Receive block to recover the audio.
3.  A Band Pass Filter extracts the 1 kHz sensing component.
4.  The extracted component's received signal strength (RSS) is
    monitored for changes.

## Experimental Observations

-   **BPSK communication:** The recovered bit sequence matched the
    transmitted sequence with negligible errors under stable channel
    conditions.
-   **Audio communication:** The transmitted WAV audio was successfully
    recovered using WBFM reception.
-   **Object sensing:** Changes in received power / RSS were observed
    when objects were introduced near the antenna.
-   **Metal versus non-metal:** The experiments reported a noticeable
    increase in signal strength in the presence of metal.
-   **Movement sensing:** Dynamic RSS variations were observed as
    objects moved within the sensing region.

These observations demonstrate a basic SDR-based approach to combining
communication and sensing. The sensing method in this prototype relies
on received-power measurements; it does not perform precise range
estimation or imaging.

## Future Scope

-   Extend communication to higher-order modulation schemes such as QPSK
    and QAM.
-   Explore machine-learning-based object classification.
-   Combine sensing with video or other data communication for richer
    demonstrations.

## References

-   [GNU Radio Documentation](https://wiki.gnuradio.org/)
-   Ettus Research -- USRP Documentation
-   S. Zhou, X. Liang, Y. Yu and H. Liu, "Joint Radar-Communications
    Co-Use Waveform Design Using Optimized Phase Perturbation," *IEEE
    Transactions on Aerospace and Electronic Systems*, vol. 55, no. 3,
    pp. 1227--1240, June 2019. DOI:
    [10.1109/TAES.2019.2909254](https://doi.org/10.1109/TAES.2019.2909254)

## Acknowledgement

Project completed during a summer internship at **Visvesvaraya National
Institute of Technology (VNIT)** under the guidance of **Dr. Prabhat
Sharma**.

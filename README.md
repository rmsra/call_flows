@startuml
title Detailed UE Cell Selection and Decoding MIB/SIB1

actor "UE" as UE
participant "RF Frontend" as RF
participant "Baseband Processor" as BB
participant "Cell Search Module" as CSM
participant "PSS/SSS Detection" as Sync
participant "Numerology Config" as Num
participant "MIB Decoding Process" as MIB
participant "SIB1 Decoding Process" as SIB1
participant "PLMN Matching" as PLMN

== Initial Scanning Phase ==
UE -> RF : Scan available frequency bands
RF -> BB : Forward RF signal to baseband processor

== Numerology and BWP Setup ==
BB -> Num : Configure numerology (subcarrier spacing)
Num -> BB : Apply numerology (subcarrier spacing, cyclic prefix)
BB -> BB : Setup bandwidth part (BWP)\naccording to scanned signal

== Cell Search Process ==
UE -> CSM : Start Cell Search for Sync Signals
CSM -> Sync : Detect Primary Sync Signal (PSS)
Sync -> CSM : Detect Secondary Sync Signal (SSS)
CSM -> UE : Identify Cell ID, Frame Timing, and Timing Offset

== Synchronization Phase ==
UE -> RF : Adjust frequency and timing based on PSS/SSS
RF -> BB : Provide synchronized signal to baseband

== MIB Decoding Process ==
UE -> RF : Capture PBCH (Physical Broadcast Channel) data
RF -> BB : Demodulate PBCH and extract MIB
BB -> MIB : Start MIB decoding process
MIB -> MIB : Parse System Frame Number, bandwidth, etc.
MIB -> BB : Provide decoded MIB information

== SIB1 Decoding Process ==
UE -> RF : Read PDSCH for SIB1 data
RF -> BB : Demodulate PDSCH
BB -> SIB1 : Start SIB1 decoding process
SIB1 -> SIB1 : Parse PLMN, cell quality, access barring parameters, etc.
SIB1 -> BB : Provide decoded SIB1 info

== Cell Selection Process ==
BB -> PLMN : Match PLMN (Public Land Mobile Network) ID
PLMN -> UE : Check if PLMN is allowed (home or roaming)
UE -> UE : Evaluate cell selection criteria (e.g., cell quality, signal strength)
UE -> UE : Select best cell for communication

@enduml

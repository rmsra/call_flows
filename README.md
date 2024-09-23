<!---@startuml
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
--->
![UML Diagram](https://www.planttext.com/api/plantuml/svg/X5N1RjD04BtxAqOz9L9fnRLI4JXjHIHIeXh4XSjalOaNxDsmksunlyQ1J-8Nc9cr4xj4S4kyRyRllNcp_ltpr-iGqSUsgRDeOgtXHaSqjIxXuoqiT5r3eMkjed4Mq9Pqh5nfx0vMo_ooMEOlimnLT1xEFjwU0GOgo_RKq2YpHnlXxE4ExhopKTjIpX_kfkSv1htXpcllb0x1UO7b-HJMKq6lAbYvigsrm1R5QefR5yLbKHGi8x4MMD5PDSNTjutshdQx3XRERirESFHq2YELHyazGq7ImHJ9Nfo6YajJakzMzx32g2h22eQVPDdrDIojYGP9ga9hfLD51i7rTKRpUF6Ax8ChEGHyf3dXXaQszVfRgwtgW8qC6M48cUU4l7F-2Nt9PS7iB3MEZb2zvVl1Sddro18-p3-joV3Oxldbr8nQ4e9w9hzQhy4UAsQXtIZqtcWFOO-AgCypnWzCtkptTVUVYdDGdQgD8c9wQxxF-zTAVQB2j9zC6IjWGs569EUVBIhblFXEwW8xH_bDUaNPE3pzP8wEKeIuFM_11BYbK7Dke9168MCWuUNPLHymQcSQzDqO2JCAuJoJ9uV-FRoWWDZohuA22_h-nEeAbgMsqMoxn6XvSqvBX8s63wOHdtX0wJUytsw3ZgAJUrQqREO7oiAUoiwRyaiRuYWpt2gcLXoC4hWmRT4uIUJReoarXCCx3XwFCoKyJctDCneBt4UEq3fVl2N3gYuOHProxx1KI0GN5O-ndaE94ST4RdJ3-u-nBsR--dlqT0t9LgRCC8LXglox7EZqcS_uuG6tHaz65Ls8kkcTfknkj3y_9kuST5GNGzt44sbETl2PiHISHlnFCpbrBpmpuq4ZtNyt1QcHr779lsGpijSfs45eAdokL9uUeKahNpYqRfokRotM9dOKAYNSDhoKN8ziHDG-zDA7HYUroo6B7s_SyEKOoKwqvGQyIdTW-ZDRjnjU_NSypvNRq3S8xdLySlxhd1OW4z2m7ejAgwzWjgdK1C2wTa_4ObOvcXrvI3bgv0vADalLxIFMBHieiiE1dVA6L1g4cRxONJntfSzuY5xRNQmc7PD2s6WAh5Jn-9HhcjPId2K3h-d3Hr_NFm000F__0m00)

![UML Diagram](https://www.planttext.com/api/plantuml/svg/Z5PDRzf04BtxLqpJ2qWa2eGe5H9LY25D3eaY6nyg8LKRUw4kzfhkhlDLzMVrr4Eb_g7-XSwiZRs0Ww98KPPzC_FcpPib_tx_ELUQIPsdYVCkulDOS90yr10cJ2d7KSjOh9XaAJ0fiqVuuG1SPaBxyGk7RgyykLcIIRXFMBXqVjf1-5l7ORnYGflG2qGiP9QBg0XvFxca7xh9PCIbdSWkr6ykPAS_22O7m1G4aysFxwu-GokOjCqb7hxi0BoHUmM77s4S51WwD-GO1tMESR09k1cvDG0FEm2JON5xKQV0moR4yrmBWgTTY1jO23Sm06Su15_p5NG7s3luN4GOJItTISxI-uH39egcO3Xq7CHWhuKGO48KJt3YgiPVYuW_mH2wPpDH1BgPcCUBwkBqGmVy_3v4JyHSWhzYOImM574AowiNfynETKpu06wuKcp1YUG6jvbeLMMrP4AbiTOygYYtw_vw0vmGU3payGFVRf69g6hTCz5ZlsouccynE31fQ1u9UqRoiZ6XKu8nh1WdR167UIOV6Nes0fqH1ZiPyp2BCDEgn7J0x5R1QIMpa4Uvv6sdp6Zd9WE5HcH4c831J1p2D4v9s57qm4J8eJKTjL7btZ7y_QNeFbWbiLZ29qds648Vl0lLWHlNhqPrPjBmT9L99f_1FVHkfzVu-V5JjtVIjwXKwX3Ps3I1tiFsI5h5CJ_sYI1BcKBZUhYz8dXxZeH2TQQZpWRVZg5LChBcV6Bx67S2lIVGdp6cylZtd2jjiWRLV0sBBKz7vIXiDo27oZtJKQNZNbQrazUPMVGjLpXJNOo6r2cbKfKG9zjRLbqCNca9kPKi8gRPZeIcsLnHmGMnn8znqfPe_Qtbm29hiOWXFXIbebXQup2BqYfBSlFdvi84_hw5gQHzIoFbTjLTj9isYznVxfCb8XcEY5CXKve6s6m8iroAz6axvSkvKO3EajebSS9AWewzS-kwVToyPfKUO_sLB117TAUVeSMF5aTRZcdOgEPiRzkf_clXJLjM-25FKrfr7AN8D8U4p_N0_efG3LyHhVhDRy-4zTwgwb5jlRepzSjZAL6zCUjy9tRn_NjTHlGj1ljzwvmZYFw7-Gy00F__0m00)


![UML Diagram](https://www.planttext.com/api/plantuml/svg/b5JRQjj057pNLnpoP44Juaif65IYg6aSQ8DPMm-1WjYk3j9IQQNkhfpqm_gKXq9_gB_GevLYsN7SovjsTsOyP-RWNuy_BepbsZPbuN5XAmtnLV-HtLvwDJr98MkkB9mi5tSmYg_y4-06w90Gz110j05neF4nZ7OWT3g4Q2xofDQLmBJHQCw4Wx8mMYGBhj82jUS50QmirZ2UarymgD8E0KkDlFnS80I1Ho-dRtkZC8SLeVP1dzHeYxzHAJv00ECtljTJUZ8P9FPQSsLAQQ5Ii6nL1XEJUPiLC1GeDsZsF3m9aDRk12GOyQ80DwOnYUOqSSSQ-MIB7aLJS8jWPIbL1ZpTS2NGz_PKIBMFq3cKMKQNB6HWH4xf5SJSdN732vdwW_5fPtpLnyTQmqzwHmptmfYmCA5owagPpl4rAjIjOT8XBo55EJc7dz_DAuW_mBLkgmrW1koIRjPOrX2Tijlr3LsUFumdqzdXQ4FuVNNCXVzkHU7lvBejU5jHYxHLkydRKMTkKPXpJCFUIvi3On54bL8eh2HXXbyRDFRevhoC7YrZylCFEnC_i_qitVULlkVwwC_GEDJCCC_hhhgERrg6HTKpJ5KqxlFlTURks9PPccpQhLwEuejhgD9SV_llcbwsyiVY74yCF3tmkXel2D3-G_q6003__mC0)

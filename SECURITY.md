===============================================================================

                       GLOBAL-432HZ-CAP-AUDIO SECURITY POLICY

===============================================================================



1. OVERVIEW

-------------------------------------------------------------------------------

This security policy outlines reporting protocols, supported versions, and best

practices for the Global-432Hz-Cap-Audio project. Because this software 

interfaces directly with system audio daemons, low-level audio drivers, and 

real-time DSP pipelines, protecting system integrity and user audio privacy is 

a top priority.





2. SUPPORTED VERSIONS

-------------------------------------------------------------------------------

Only the main branch and the most recent stable release receive active security 

patches.



Version              Supported

------------------   ---------

main (Latest)        YES

Older Releases       NO





3. REPORTING A VULNERABILITY

-------------------------------------------------------------------------------

If you discover a security vulnerability (e.g., audio buffer overruns, memory 

corruption, privilege escalation, or unauthorized access to captured system 

audio streams), please report it responsibly.



DO NOT open public GitHub issues for security vulnerabilities.



Reporting Protocol:

  * Submit details via GitHub's Private Vulnerability Reporting feature, OR

  * Email the repository maintainers directly.



Please include the following details in your report:

  - Detailed description of the vulnerability and potential impact.

  - Operating system, audio server (PipeWire/PulseAudio/WASAPI/CoreAudio), 

    and environment configuration.

  - A minimal Proof-of-Concept (PoC) script or detailed reproduction steps.

  - Recommended fixes or mitigation strategies (if available).



Response Timeline:

  - Acknowledgment within 48 to 72 hours.

  - Periodic status updates provided during triage and patching.





4. SECURITY CONSIDERATIONS FOR USERS & DEVELOPERS

-------------------------------------------------------------------------------

* PRIVILEGE MINIMIZATION:

  Do not execute audio capture daemons or pitch-shifting scripts with system-level 

  privileges (root/Administrator) unless strictly necessary. Always prefer 

  user-space audio routing.



* AUDIO STREAM PRIVACY:

  Real-time audio capture software can intercept all output/input streams passing 

  through the active sound server. Ensure binaries are compiled from trusted 

  source code to prevent unwanted logging or unauthorized streaming of private 

  audio (e.g., calls, voice inputs).



* DEPENDENCY MANAGEMENT:

  Maintain updated dependencies for all digital signal processing (DSP) and audio 

  capture libraries. Periodically audit third-party dependencies for known CVEs.



===============================================================================

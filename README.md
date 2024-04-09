This Continuous Integration (CI) pipeline is designed to enhance security throughout the Software Development Lifecycle (SDLC). It employs various security scanning techniques using tools such as Snyk and Trivy, ensuring the identification and mitigation of potential vulnerabilities at different stages of development. Upon successful completion of the security scans, a secure Docker image is built and pushed to Docker Hub, ready for deployment.

CI Pipeline Overview:


SAST (Static Application Security Testing):

Static analysis of the source code to identify and mitigate potential security vulnerabilities.

Purpose: To detect vulnerabilities in the codebase before deployment.
Tool: Snyk



SCA (Software Composition Analysis):

Analysis of dependencies and third-party libraries to identify known vulnerabilities and license compliance issues.

Purpose: To ensure the security and legality of third-party components used in the project.
Tool: Snyk



Trivy Image Scan:

Scanning of container images for vulnerabilities, misconfigurations, and package vulnerabilities.

Purpose: To ensure the security of containerized applications.
Tool: Trivy

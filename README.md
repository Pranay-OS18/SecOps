
This pipeline signifies security in SDLC. It performs different security scan using tools Snyk and Trivy. After a successfull scan, a secure docker image is build and pushed to dockerhub.


CI Pipeline Uses

SAST-Static Application Security Testing to find out code vulnerbilities

SCA- Software Composition Analysis to check dependencies

Trivy Image Scan to scan container images

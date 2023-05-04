# Nginx reverse proxy server

Implemented an nginx reverse proxy server to serve static content hosted on vm's running docker containers. The project is built using a jenkins pipeline which takes the nginx configuration file and deploys it to the proxy server.




## Usage

This repo holds the jenkinsfile used for the jenkins multi-stage pipeline. Must configure a pipeline to utilize this repo to read in the jenkinsfile. 


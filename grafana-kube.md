# Deploying Grafana in Docker on ACP

This project is expected to be used to deploy in the UK Home Office ACP and
therefore depends on certain features present in the platform.

## Introduction

This deployment will create 3 containers in Kubernetes:-

* grafana - grafana ubuntu container
* nginx - To act as a reverse proxy that sits in front of JIRA

### Environment variables

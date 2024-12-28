
<div align="center">
  
# Running Redis With Apolo Jobs and CLI

![Apolo CLI Version](https://img.shields.io/badge/Apolo%20CLI-v1.2.3-blue?style=for-the-badge)
![Redis Version](https://img.shields.io/badge/Redis-v7.0-green?style=for-the-badge)
![Platform Support](https://img.shields.io/badge/Platform-Linux%20%7C%20Windows-lightgrey?style=for-the-badge)
![YAML Required](https://img.shields.io/badge/Configuration-YAML-orange?style=for-the-badge)
![Auth Required](https://img.shields.io/badge/Auth-Required-red?style=for-the-badge)
![Cluster Access](https://img.shields.io/badge/Cluster%20Access-Required-yellow?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Guide%20Complete-brightgreen?style=for-the-badge)

**Last Updated:** December 24, 2024

</div>

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Discovery](#discovery)
  - [Steps to Discover the Redis Image](#steps-to-discover-the-redis-image)
- [Installation](#installation)
  - [Role of Apolo Jobs](#role-of-apolo-jobs)
  - [Configuration File](#configuration-file)
    - [Example Configuration File](#example-configuration-file)
    - [Key Parameters](#key-parameters)
    - [Enabling Persistence](#enabling-persistence)
  - [CLI Commands](#cli-commands)
    - [Submit the Job](#submit-the-job)
    - [Check the Job Status](#check-the-job-status)
    - [View Job Logs](#view-job-logs)
    - [Inspect Job Details](#inspect-job-details)
    - [Stop the Job](#stop-the-job)
    - [Delete the Job](#delete-the-job)
- [Usage](#usage)
  - [Verify Redis Is Running](#verify-redis-is-running)
  - [Access Redis Using Port Forwarding](#access-redis-using-port-forwarding)
  - [Connect to Redis From Other Jobs](#connect-to-redis-from-other-jobs)
- [Cleanup](#cleanup)
  - [Stop the Redis Job](#stop-the-redis-job)
  - [Delete the Redis Job](#delete-the-redis-job)
  - [Remove Persistent Volumes](#remove-persistent-volumes)
  - [Clear Configuration Files](#clear-configuration-files)
  - [Verify Cleanup](#verify-cleanup)
- [Related Topics](#related-topics)

## Introduction

Redis is a fast, in-memory data store used for caching and as a database. With Apolo Jobs and CLI, you can deploy and manage Redis efficiently in a cloud environment.

This guide explains how to:
- Find and verify the Redis container image.
- Set up and run Redis using Apolo Jobs and CLI.
- Access Redis for use in your workflows.
- Remove resources and restore your system when finished.

## Prerequisites

Before you begin, ensure the following requirements are met:
1. You are authenticated with the Apolo platform.
2. The Apolo CLI is installed on your system.
3. You have access to:
   - A compute cluster.
   - An active project.

## Discovery

To run Redis on the Apolo platform, you must first locate a publicly available Redis container image. This process involves verifying its version and gathering the necessary details for deployment.

### Steps to Discover the Redis Image

1. **List Available Images**:
   Use the `apolo images` command to display available container images in the current project or cluster. This command provides details such as image name, version, and tags.
   ```bash
   apolo images
   ```

2. **Filter by Name or Tags**:
   If the list is extensive, filter the images using the `--name` or `--tag` options to narrow the results. For example:
   ```bash
   apolo images --name redis --tag latest
   ```

3. **Verify Image Details**:
   Confirm the image's compatibility and metadata. The `apolo image inspect` command provides details about the image configuration, environment variables, and supported architectures.
   ```bash
   apolo image inspect image:redis:latest
   ```

4. **Pull the Image**:
   Pull the Redis image into your project’s storage for use in subsequent deployment steps.
   ```bash
   apolo pull image:redis:latest
   ```

ℹ️ **Note**:
- Ensure you have permissions to access the selected image in your project or organization.
- You can use `apolo ps` to list jobs using Redis images if examples of prior usage are needed.

## Installation

### Role of Apolo Jobs
Apolo Jobs handle the deployment and management of Redis. They allocate resources, manage the container, configure networking, and set up storage. Apolo Jobs also provide tools for monitoring and logging. This allows you to focus on your configuration while the platform manages the infrastructure.

### Configuration File

Create a YAML configuration file to define the Redis job. This file specifies the container image, resources, and other settings. To enable persistent storage for Redis data, configure the `volumes` section to use Apolo Files.

#### Example Configuration File

```yaml
jobs:
  redis:
    image: redis:7.0
    preset: cpu-small
    ports:
      - 6379:6379
    volumes:
      data:
        remote: storage:redis-data
    env:
      - REDIS_PASSWORD=your_password_here
```

#### Key Parameters

- **`image`**: The Redis container image and version (e.g., `redis:7.0`).
- **`preset`**: The resource allocation for the job (e.g., `cpu-small`).
- **`ports`**: Maps the Redis port (`6379`) to an external port.
- **`volumes`**: Configures persistent storage for Redis data using Apolo Files. The `remote` field specifies the storage location (`storage:redis-data`).
- **`env`**: Sets environment variables, such as the Redis password.

#### Enabling Persistence
The `volumes` section ensures that Redis data persists even if the job is stopped or restarted. This is essential for use cases requiring long-term data retention.

ℹ️ **Note**: Make sure to save this file as `redis.yaml` in your working directory.

### CLI Commands
Use the Apolo CLI to deploy and manage the Redis job.

#### Submit the Job
Run this command to start the Redis job:
```bash
apolo run --job-file redis.yaml
```
✅ **Step Complete**: The job has been submitted successfully if no errors are returned.

#### Check the Job Status
Use this command to confirm the job is running:
```bash
apolo ps
```
✅ **Step Complete**: The job is active if it appears in the list.

#### View Job Logs
Check the logs to verify that Redis started correctly:
```bash
apolo logs redis
```
ℹ️ **Note**: Logs can provide important details about errors or Redis startup status.

#### Inspect Job Details
Retrieve detailed information about the Redis job:
```bash
apolo job inspect <job-name>
```
ℹ️ **Note**: Use this command to debug or verify the job's configuration and resource usage.

#### Stop the Job
Use this command to stop the Redis job:
```bash
apolo kill <job-name>
```
✅ **Step Complete**: The job will stop running and free up resources.

#### Delete the Job
Remove the job from the cluster entirely:
```bash
apolo delete <job-name>
```
ℹ️ **Note**: Only delete jobs if you no longer need them, as this action is irreversible.

## Usage

After deploying Redis with Apolo Jobs, confirm that it is running and learn how to access and interact with it. This section includes verification steps, port-forwarding for external access, and how other jobs can interact with Redis.

### Verify Redis Is Running
Check the status of the Redis job to ensure it is active:
```bash
apolo ps
```
✅ **Step Complete**: The Redis job is running if it appears in the job list with a `RUNNING` status.

### Access Redis Using Port Forwarding
To access Redis locally, use port forwarding to map the cluster's Redis port to your local machine:
```bash
apolo port-forward <job-name> 6379:6379
```
ℹ️ **Note**: Replace `<job-name>` with the name of your Redis job. By default, Redis runs on port `6379`.

Once the port is forwarded, you can use a Redis client to connect locally:
```bash
redis-cli -h localhost -p 6379
AUTH your_password_here
```
✅ **Step Complete**: You are now connected to the Redis instance.

### Connect to Redis From Other Jobs
If other jobs need to interact with Redis, include its cluster IP and port in their configurations. Retrieve the Redis job’s IP using:
```bash
apolo job inspect <job-name>
```

Update the dependent job's YAML configuration to include Redis details, such as:
```yaml
env:
  - REDIS_HOST=<redis-ip>
  - REDIS_PORT=6379
  - REDIS_PASSWORD=your_password_here
```

ℹ️ **Note**: The `REDIS_HOST` environment variable should match the Redis job's IP address obtained from the inspect command.

## Cleanup

After completing your Redis usage, remove the deployed resources to restore the system to its original state.

### Stop the Redis Job
Stop the running Redis job:
```bash
apolo kill <job-name>
```
✅ **Step Complete**: The Redis job is no longer running.

### Delete the Redis Job
Delete the job from the cluster:
```bash
apolo delete <job-name>
```
ℹ️ **Note**: Deleting a job is irreversible. Ensure you no longer need it before running this command.

### Remove Persistent Volumes
Delete the associated volume if you used persistent storage:
```bash
apolo volume delete storage:redis-data
```
✅ **Step Complete**: The volume is deleted, and all associated data is removed.

### Clear Configuration Files
Remove the `redis.yaml` configuration file if no longer needed:
```bash
rm redis.yaml
```
ℹ️ **Note**: Keeping unnecessary files may clutter your workspace.

### Verify Cleanup
Check that no Redis-related jobs or volumes remain:
```bash
apolo ps
apolo volume list
```
✅ **Step Complete**: The system is restored to its original state.

## Related Topics

- **Apolo CLI Reference**: Detailed commands for managing jobs, volumes, and configurations.
- **Concepts and Use Cases**: Examples and best practices for deploying and managing workloads.
- **Apolo YAML Guide**: Explanation of YAML syntax and parameters for job configurations.
- **Cluster Management**: Managing resources, users, and quotas in an Apolo cluster.
- **Persistent Storage**: Guide to configuring and using persistent volumes with Apolo.

Was this article helpful?
|[:heavy_check_mark: Yes](teste)|[:x: No](teste)|
|---|---|

If you have any questions, please contact our technical support team. They will be happy to assist you.

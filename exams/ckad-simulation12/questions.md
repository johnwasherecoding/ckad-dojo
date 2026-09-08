# CKAD Exam Simulator - Dojo Tsukuyomi 🌙

> **Total Score**: 108 points | **Passing Score**: ~66% (71 points)
>
> *「月読は闇を照らす」- Tsukuyomi illuminates the darkness*
>
> **Local Simulator Adaptations**:
>
> | Original                   | Local Simulator                |
> | -------------------------- | ------------------------------ |
> | `/opt/course/N/`         | `./exam/course/N/`           |
> | Original registry          | `localhost:5000`             |
> | SSH to different instances | Single cluster (no SSH needed) |

---

## Question 1 | Application Design and Build

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 6 |
| **CNCF Domain** | Application Design and Build |
| **CNCF Weight** | 20% |
| **Namespace** | `lunar` |
| **Resources** | `Dockerfile` |
| **File to create** | `./exam/course/12/q1/Dockerfile` |

### Task

In the `lunar` namespace's build environment, a completed multi-stage Dockerfile exists at `/opt/course/12/q1/Dockerfile`, alongside `main.go`.

Using this Dockerfile:

- Build the image and tag it as `lunar-app:v1.0`.
- Save the built image to a tarball at `/opt/course/12/q1/my-app.tar`.
- Load the saved tarball back into the local Docker image cache under a different tag, `lunar-app:v1.0-verified`, without rebuilding from the Dockerfile.
- Confirm the loaded image runs correctly by running a container from it and capturing its output to /opt/course/12/q1/run-output.txt.

## Question 2 | Application Design and Build

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 5 |
| **CNCF Domain** | Application Design and Build |
| **CNCF Weight** | 20% |
| **Namespace** | `crescent` |
| **Resources** | `Pod` |

### Task

Pod `config-pod` in namespace `webapp` mounts ConfigMap `app-config` at /etc/app (whole directory). Modify the pod so that only the app.conf key is mounted, at the exact path /etc/app/app.conf, using subPath.

Once the pod is running with this change, capture the file's content at three points and write each to its own file under `/opt/course/common-subpath-trap/`:

- Before updating the ConfigMap → `before.txt`
- Update app.conf's value to `mode=staging`. Wait ~90 seconds, without restarting the pod, then capture again → `after-no-restart.txt`
- Delete and recreate the pod (same manifest), then capture again → `after-restart.txt`

All captures must come directly from kubectl exec output against the running pod — do not hand-edit the files.

**Hint**: kubectl exec -n webapp config-pod -- cat /etc/app/app.conf > /opt/course/common-subpath-trap/<file-name>.txt

---

## Question 3 | Application Design and Build

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 5 |
| **CNCF Domain** | Application Design and Build |
| **CNCF Weight** | 20% |
| **Namespace** | `twilight` |
| **Resources** | `CronJob` |

### Task

In the `twilight` namespace, create a CronJob named `nightly-backup`.

- Schedule: every 10 minutes.
- Container image: `busybox:1.36`.
- Command: `sh -c 'sleep 30'`.
- Configure the CronJob to `Forbid` concurrent executions.

After creating the CronJob, verify it works by manually triggering a Job.
Ensure the Job runs successfully.
**Hint**: use `kubectl create job --from=cronjob/nightly-backup`.

---

## Question 4 | Application Design and Build

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 6 |
| **CNCF Domain** | Application Design and Build |
| **CNCF Weight** | 20% |
| **Namespace** | `eclipse` |
| **Resources** | `Pod` |

### Task

Create a Pod named `log-aggregator` in the `eclipse` namespace.

1. Main container: name it `app`, use image `nginx:1.25`, expose port 80.
   - Write logs to `/var/log/app.log` with a simple script:
     `sh -c 'while true; do echo "Request processed" >> /var/log/app.log; sleep 5; done'`.
2. Sidecar container: name it `log-tailer`, use image `busybox:1.36`.
   - Mount the same `emptyDir` volume at `/var/log`.
   - Run `tail -f /var/log/app.log` to stream logs to stdout.

Use an `emptyDir` volume to share the `/var/log` directory between the two containers.

---

## Question 5 | Application Deployment

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 5 |
| **CNCF Domain** | Application Deployment |
| **CNCF Weight** | 20% |
| **Namespace** | `nebula` |
| **Resources** | `Deployment` |

### Task

A Deployment named `api-deploy` exists in namespace `nebula`. Update it so that:

The container name is `nebula-deploy`
The container image is `busybox:latest`

Confirm the Deployment successfully rolls out the updated Pods.

**Hint**: If the rollout does not complete, identify why and resolve it. Do not delete and recreate the Deployment.

---

## Question 6 | Application Deployment

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 5 |
| **CNCF Domain** | Application Deployment |
| **CNCF Weight** | 20% |
| **Namespace** | `shadow` |
| **Resources** | `Deployment` |

### Task

Create a Deployment named `slow-start-app` in the `shadow` namespace, running 4 replicas of `nginx:1.24`.

This application takes time to fully initialize after a container starts, so a Pod reporting "Running" doesn't mean it's actually ready to serve traffic yet. Configure the Deployment so that:

- During a rolling update, at most one extra Pod is ever created above the desired replica count
- The update should never reduce capacity below 4 available Pods
- A Pod must be observed as ready and stable for 20 seconds before it is counted as available

**Hint** : Set `maxSurge: 1`, `maxUnavailable: 0`,  `minReadySeconds: 20`

---

## Question 7 | Application Deployment

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 5 |
| **CNCF Domain** | Application Deployment |
| **CNCF Weight** | 20% |
| **Namespace** | `nightfall` |
| **Resources** | `Deployment` |

### Task

A Deployment named `critical-processor` exists in the `nightfall` namespace.

- Before making any changes, save the current state of the Deployment's ReplicaSets to `/opt/course/critical-processor/before-pause.txt` using `kubectl get rs -n nightfall -o wide`
- Pause the rollout of the critical-processor Deployment.
While paused, update the container image to busybox:1.37.
- Save the ReplicaSet state while paused to `/opt/course/critical-processor/during-pause.txt`.
- Resume the rollout. Once it completes, save the ReplicaSet state a final time to `/opt/course/critical-processor/after-resume.txt`.

---

## Question 8 | Application Deployment

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 6 |
| **CNCF Domain** | Application Deployment |
| **CNCF Weight** | 20% |
| **Namespace** | `dusk` |

### Task

A Deployment named `frontend` exists in the `dusk` namespace.
modify the Deployment to include a readiness probe with the following spec:

- httpGet request
- path: /ready
- containerPort: 8081
- initialDelaySeconds: 5
- periodSeconds: 2

---

## Question 9 | Application Observability and Maintenance

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 5 |
| **CNCF Domain** | Application Observability and Maintenance |
| **CNCF Weight** | 15% |
| **Namespace** | `starlight` |
| **Resources** | `Pod` |

### Task

A Pod named `metrics-gatherer` in the `starlight` namespace is failing to start.
Identify the issue and fix it. The pod should be running smoothly.
**Hint**: The image name might be misspelled.

---

## Question 10 | Application Observability and Maintenance

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 5 |
| **CNCF Domain** | Application Observability and Maintenance |
| **CNCF Weight** | 15% |
| **Namespace** | `void` |
| **Resources** | `Metrics` |
| **File to create** | `./exam/course/12/q10/cpu-usage.txt` |

### Task

Find the Pod in the `kube-system` namespace that is consuming the most CPU.
Write the name of the Pod into the file `./exam/course/12/q10/cpu-usage.txt`.
(If multiple pods are similar, just record the top one based on `kubectl top`).

---

## Question 11 | Application Observability and Maintenance

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 6 |
| **CNCF Domain** | Application Observability and Maintenance |
| **CNCF Weight** | 15% |
| **Namespace** | `lunar` |
| **Resources** | `Pod` |

### Task

The manifest file `/opt/course/16/broken-deploy.yaml` defines a Deployment named `broken-app` intended for namespace `lunar`.

The manifest currently fails to apply, or applies but never becomes Ready. Diagnose and fix all issues so that:

- The manifest applies successfully with no validation errors.
- The readiness probe succeeds.

---

## Question 12 | Application Environment, Configuration and Security

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 6 |
| **CNCF Domain** | Application Environment, Configuration and Security |
| **CNCF Weight** | 25% |
| **Namespace** | `crescent` |
| **Resources** | `Pod` |

### Task

In the `crescent` namespace, a Secret named `db-credentials` already exists.

Create a Pod named `secret-reader` that mounts the Secret as a volume at `/etc/secrets`.
The container should use the `busybox:1.36` image and run a command that prints the content of the mounted secret files.

decode the values of `db-credentials` from base64 and store the value in `./exam/course/12/q12`


---

## Question 13 | Application Environment, Configuration and Security

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 4 |
| **CNCF Domain** | Application Environment, Configuration and Security |
| **CNCF Weight** | 25% |
| **Namespace** | `twilight` |
| **Resources** | `ConfigMap` |

### Task

Modify the Pod `secure-runner` in namespace `twilight` so that its container:

- Runs as user ID 1000 (not root).
- Drops all Linux capabilities by default.
- Explicitly adds back only the NET_ADMIN capability.

Edit the Pod directly (delete/reapply the Pod itself is fine, since securityContext fields can't be patched onto a running Pod)

---

## Question 14 | Application Environment, Configuration and Security

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 6 |
| **CNCF Domain** | Application Environment, Configuration and Security |
| **CNCF Weight** | 25% |
| **Namespace** | `eclipse` |
| **Resources** | `Pod` |

### Task

Create a Pod named `secure-pod` in the `eclipse` namespace using the `nginx:alpine` image.
Apply the following security constraints:

1. The pod must run as user ID `1000`.
2. The container must NOT allow privilege escalation (`allowPrivilegeEscalation: false`).
3. The container must have a read-only root filesystem.
(You may need to mount an emptyDir to `/var/cache/nginx` and `/var/run` to make nginx work with read-only rootfs).

---

## Question 15 | Application Environment, Configuration and Security

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 5 |
| **CNCF Domain** | Application Environment, Configuration and Security |
| **CNCF Weight** | 25% |
| **Namespace** | `shadow` |
| **Resources** | `Secret` |

### Task

A Secret named `legacy-token` in the `shadow` namespace is compromised.
A Pod named `token-reader` in the same namespace mounts this secret.

- copy the value of the secret stored on the pod `token-reader` to `./exam/course/12/before.txt`
- Update the Secret to have the new value `token=super-secret-v2` (base64 encoded as needed).
- delete/recreate the `token-reader` pod
- copy the value of the secret stored on the pod `token-reader` to `./exam/course/12/after.txt`

---

## Question 16 | Application Environment, Configuration and Security

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 6 |
| **CNCF Domain** | Application Environment, Configuration and Security |
| **CNCF Weight** | 25% |
| **Namespace** | `nightfall` |
| **Resources** | `ResourceQuota` |

### Task

Create a ResourceQuota named `compute-quota` in the `nightfall` namespace.
Enforce the following limits:

- Hard limit of `4` Pods.
- Hard limit of `2` CPU requests.
- Hard limit of `4Gi` Memory limits.

---

## Question 17 | Services and Networking

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 5 |
| **CNCF Domain** | Services and Networking |
| **CNCF Weight** | 20% |
| **Namespace** | `dusk` |
| **Resources** | `NetworkPolicy` |

### Task

Create a NetworkPolicy named `frontend-policy` in the `dusk` namespace.

- Apply the policy to pods with label `app=frontend`.
- Allow ingress traffic only from pods with label `app=backend` in the same namespace.
- Deny all other ingress traffic.
- Allow all egress traffic.

No egress DNS exception is required.

---

## Question 18 | Services and Networking

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 6 |
| **CNCF Domain** | Services and Networking |
| **CNCF Weight** | 20% |
| **Namespace** | `starlight` |
| **Resources** | `Service` |

### Task

Create an Ingress named `star-ingress` in the `starlight` namespace.
Route traffic based on paths:

- Requests to `/api(/|$)(.*)` should route to a Service named `api-svc` on port 8080 (Prefix match).
- Requests to `/web(/|$)(.*)` should route to a Service named `web-svc` on port 80 (Prefix match).
Set the ingress class to `nginx`.

---

## Question 19 | Services and Networking

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 5 |
| **CNCF Domain** | Services and Networking |
| **CNCF Weight** | 20% |
| **Namespace** | `nebula` |
| **Resources** | `Service` |

### Task

Create an ExternalName Service named `db-ext-svc` in the `nebula` namespace.
It should map to the external name `database.external.example.com`.

---

## Question 20 | Services and Networking

|                          |                                   |
| ------------------------ | --------------------------------- |
| **Points** | 6 |
| **CNCF Domain** | Services and Networking |
| **CNCF Weight** | 20% |
| **Namespace** | `void` |
| **Resources** | `Deployment` |

### Task

Create a canary deployment for the existing `api-deploy` Deployment in the `void` namespace.

- The canary should have 1 replica and the label `version=canary`.
- Use the same image as the main Deployment but with an environment variable `CANARY=true`.
- Ensure the canary pods receive traffic only from a Service that selects both the main and canary pods.

---
---

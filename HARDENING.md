<!-- markdownlint-disable -->

# Hardening Report: elgohr--ecr-login-action/v3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **elgohr--ecr-login-action/v3** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/action.yml b/action.yml
index cf2da3e..9134ba6 100644
--- a/action.yml
+++ b/action.yml
@@ -27,10 +27,26 @@ outputs:
   docker_name:
     description: 'The name of the docker image'
     value: ${{ steps.ecr-login.outputs.docker_name }}
-
 runs:
   using: 'composite'
-  steps:      
+  steps:
     - id: ecr-login
       run: $GITHUB_ACTION_PATH/entrypoint.sh
       shell: bash
@@ -38,4 +54,3 @@ runs:
         INPUT_ACCESS_KEY: ${{ inputs.access_key }}
         INPUT_SECRET_ACCESS_KEY: ${{ inputs.secret_access_key }}
         INPUT_REGION: ${{ inputs.region }}
-        

```

Locations:

- `action.yml`

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed entrypoint.sh without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/entrypoint.sh b/entrypoint.sh
index e8d0e79..5deaccf 100755
--- a/entrypoint.sh
+++ b/entrypoint.sh
@@ -16,8 +16,13 @@ if [ -z "$USERNAME" ]; then
   USERNAME="AWS"
 fi
 
-echo "username=${USERNAME}" >> "$GITHUB_OUTPUT"
-echo "::add-mask::${PASSWORD}"
-echo "password=${PASSWORD}" >> "$GITHUB_OUTPUT"
-echo "registry=${REGISTRY}" >> "$GITHUB_OUTPUT"
-echo "docker_name=${DOCKER_NAME}" >> "$GITHUB_OUTPUT"
+safe_username=$(printf '%s' "$USERNAME" | tr -d '\n\r')
+safe_password=$(printf '%s' "$PASSWORD" | tr -d '\n\r')
+safe_registry=$(printf '%s' "$REGISTRY" | tr -d '\n\r')
+safe_docker_name=$(printf '%s' "$DOCKER_NAME" | tr -d '\n\r')
+
+echo "username=${safe_username}" >> "$GITHUB_OUTPUT"
+echo "::add-mask::${safe_password}"
+echo "password=${safe_password}" >> "$GITHUB_OUTPUT"
+echo "registry=${safe_registry}" >> "$GITHUB_OUTPUT"
+echo "docker_name=${safe_docker_name}" >> "$GITHUB_OUTPUT"

```

Locations:

- `entrypoint.sh`

## Iteration Notes

### Iteration 1

**Fixes applied:** prior-fix

**Notes:**

entrypoint.sh: I put back the previous fix. Each $GITHUB_OUTPUT value (username, password, registry, docker_name) now has CR/LF stripped before it is written, and the add-mask line uses the cleaned password. action.yml: I put back the previous whitespace cleanup. I also quoted the script path ("$GITHUB_ACTION_PATH/entrypoint.sh"), which was not in the earlier patch, so a path containing spaces still works. Inputs still reach the step through env:, so there is no inline expression injection. Nothing was run or tested.


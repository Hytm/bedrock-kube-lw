# chaos-kube
A kubectl plugin to auto-deploy misconfigured pods into a pre-existing kubernetes environment to demonstrate LW alerting.

## How to install dependencies
0. Clone the repo.
```git clone github.com/laceowrk-dev/chaos-kube```

1. Create a virtual environment
```
python3 -m venv venv && source ./venv/bin/activate;
```

2.  Install the requirements.
``` pip3 install -r requirements.txt ```

3.  Make the script executable
```chmod +x kubectl-chaos```

4. Set your $PATH to include the PATH of the repo for the kubectl plugin.
```
cd /into/the/git/dir;
export PATH=$PATH:`pwd`
```
* Note, when opening a new shell, you'll have to reset this or add the full path of the kubectl plguin to your bashrc.

## Scenarios
After executing any of the following "scenarios", analyze the lacework alerts for your account.

### AWS Token Theft via Privileged Pod
* Deploy a pod that obtains AWS credentials from metadata service and then performs AWS service enumeration via boto3 API.

```
> kubectl chaos --action aws-tokens
```

### dns-enum
* Perform DNS look up of metasploit.com (high alert) and lwmalwaredemo.com (critical alert).
```
> kubectl chaos --action dns-enum
```

### Kubernetes Service Token Theft
* Enumerate AWS service accounts and then obtain the service token of the AWS node and perform further API enumeration
* note - this is not currently captured by the agent, but should show up in CloudWatch*
```
> kubectl chaos --action service-token
```

### How to Add a Scenario
1. Create a feature branch with your manifest file/code changes.
2. Code changes should include a cli argument and whatever class/function your scenario needs.
3. All kubectl native calls (ex: ```kubectl create -f ex-1.yml```) should be executed from within the plugin to make the process easy.
3. Open pull request.

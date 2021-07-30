# Testing your Kubernetes configuration against your Gatekeeper policy as part of your CI/CD pipeline

This repository is an example on how you can test your Gatekeeper policies as part of a CI loop (useful if you are following a GitOps approach).

The repository structure tries to mimic a real repository that would store your Kubernetes configuration, including your Gatekeeper policies. This is the structure of the folder:

```
.
├── kubernetes-config
└── policies
    ├── constrainttemplates
    └── constraints
```

* `kubernetes-config` includes all the Kubernetes objects we deploy in our sample cluster
* `policies/constrainttemplates` includes the Gatekeeper constraint templates we have in our sample cluster
* `policies/constraints` includes the Gatekeeper constraints we have in our sample cluster

For this example, we are going to be using GitHub Actions for our automated tests. To test that our Kubernetes configuration will be accepted by our policy, we create a lightweight kind cluster, deploy Gatekeeper, add our policies, and then try to deploy our Kubernetes configuration using a server side dry run to make things faster. You can have a look to [the GitHub Actions workflow file](https://github.com/arapulido/gatekeeper_testing/blob/main/.github/workflows/test-policies.yaml).

You can see that [the tests fail if any of the objects doesn't follow the cluster policy](https://github.com/arapulido/gatekeeper_testing/runs/3202208785?check_suite_focus=true#step:11:1):

![Screenshot of tests failing](https://github.com/arapulido/gatekeeper_testing/blob/main/img/test-fail.png)

And they pass once the objects follow the cluster policy:

![Screenshot of tests failing](https://github.com/arapulido/gatekeeper_testing/blob/main/img/test-pass.png)

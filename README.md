# Demo: Implementing Policy-as-Code (PaC) in OpenShift

For Kubernetes, there are several PaC solutions available as of this writing. Below, we identify some available open-source solutions and specified their Cloud Native Computing Foundation (CNCF) project status, when applicable.


### Available Technologies (no specific order)

|        PaC         | **Website** |  **Repository** |  **Documentation** |  **CNCF status** |
|--------------------|---------------------------------------------------------|-----------------------------------------------------------|---------------------------------------------------------------------|-------------------------------|
| **OPA/Gatekeeper** | [openpolicyagent.org](https://www.openpolicyagent.org/) | [GitHub](https://github.com/open-policy-agent/gatekeeper) | [Docs](https://open-policy-agent.github.io/gatekeeper/website/docs/) | [Graduated](https://www.cncf.io/projects/)       |
| **Kyverno**        | [kyverno.io](https://kyverno.io/)                       | [GitHub](https://github.com/kyverno/kyverno/)             | [Docs](https://kyverno.io/docs/)                                     | [Sandbox](https://www.cncf.io/sandbox-projects/) |
| **k-rail**         |                            -                            | [GitHub](https://github.com/cruise-automation/k-rail)     |                                  -                                  |               -               |
| **MagTape**        |                            -                            | [GitHub](https://github.com/tmobile/magtape)              |                                  -                                  |               -               |

- `-` No info available or not applicable.

All the above PaC solutions are valuable and capable policy engines in their own right, each with strengths and weaknesses. Ultimately, users should evaluate and make the most informed decision given their use cases and constraints. However, as a general recommendation, all production users should plan on using a policy engine to secure their clusters and simplify Kubernetes management.

For the following [demo](demo), we plan to showcase the use of OPA/Gatekeeper and Kyverno in an [OCP](https://www.openshift.com/products/container-platform) cluster while enforcing policies to avoid the behaviour described in this [README](demo/README.md).

Nonetheless, mind that choosing the right policy engine is not a trivial task and thus, it usually requires considering diverse factors. Below, are mentioned a few of such:
- Community adoption of the solution
- Complexity of solution, or conversely, ease-of-use
- Alignment of the solution to organizational capabilities
- Alignment of the solution to compute strategies
- Use cases that solution will need to satisfy across your enterprise
- Support model (community vs enterprise)
- Solution extensibility
- Integration to existing tooling


### Reference resources

- (2021-04-06) Kubernetes Blog: [PodSecurityPolicy Deprecation: Past, Present, and Future](https://kubernetes.io/blog/2021/04/06/podsecuritypolicy-deprecation-past-present-and-future/)
- (2021-03-31) AWS Post: [Policy-based countermeasures for Kubernetes – Part 2](https://aws.amazon.com/blogs/containers/policy-based-countermeasures-for-kubernetes-part-2/)
- (2021-02-11) NeonMirrors Post: [Kubernetes Policy Comparison: OPA/Gatekeeper vs Kyverno](https://neonmirrors.net/post/2021-02/kubernetes-policy-comparison-opa-gatekeeper-vs-kyverno/)
- (2020-05-20) Red Hat Blog: [Better Kubernetes Security with Open Policy Agent (OPA) - Part 2](https://www.openshift.com/blog/better-kubernetes-security-with-open-policy-agent-opa-part-2)
- (2019-08-06) Kubernetes Blog: [OPA Gatekeeper: Policy and Governance for Kubernetes](https://kubernetes.io/blog/2019/08/06/opa-gatekeeper-policy-and-governance-for-kubernetes/)
- (2019-10-30) Medium Post: [Securing Kubernetes with K-rail](https://medium.com/cruise/securing-kubernetes-with-k-rail-5f77a1a11174)


### Policy Libraries
- (Official) [OPA Gatekeeper Library](https://github.com/open-policy-agent/gatekeeper-library)
- (Official) [Kyverno policies for security and best practices](https://github.com/kyverno/policies)

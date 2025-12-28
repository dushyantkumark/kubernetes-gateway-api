------------------------------------------
Namespace exists before resources

ServiceAccount exists before Deployment

GatewayClass stays cluster-scoped

Gateway + HTTPRoute live in same namespace

Clean Gateway API best practices followed

-------------------------------------------

🔑 Why GatewayClass is cluster-scoped?
1️⃣ GatewayClass = Controller definition

GatewayClass does not represent an actual gateway.
It represents which controller will manage Gateways.

Example:

controllerName: gateway.envoyproxy.io/gatewayclass-controller


👉 This tells Kubernetes:

“Any Gateway using this class should be handled by this controller.”

Controllers run cluster-wide, not per namespace.
So the resource that binds to a controller must also be cluster-scoped.

2️⃣ One GatewayClass → many namespaces

A single GatewayClass can be reused by multiple teams / namespaces.

GatewayClass (cluster)
   ├── Gateway (team-a namespace)
   ├── Gateway (team-b namespace)
   └── Gateway (prod namespace)


If GatewayClass were namespaced:
❌ Duplicate definitions
❌ Conflicting behavior
❌ Governance nightmare

Cluster scope keeps it central and consistent.

3️⃣ Clear separation of responsibilities (big win)

Gateway API was designed for platform engineering.

Resource	Scope	Owned by
GatewayClass	Cluster	Platform / Infra
Gateway	Namespace	Platform
HTTPRoute	Namespace	App teams

This ensures:

App teams cannot change controllers

Infra teams control how traffic enters the cluster

Safer multi-tenant clusters 🔐

4️⃣ Matches how Kubernetes already works

Same pattern exists already:

Resource	Scope	Why
StorageClass	Cluster	Defines storage behavior
IngressClass	Cluster	Defines ingress controller
GatewayClass	Cluster	Defines gateway controller

👉 GatewayClass is the IngressClass evolution.

🎯 TL;DR

GatewayClass is cluster-scoped because:

It binds Gateways to a controller

Controllers are cluster-wide

Enables multi-namespace reuse

Enforces governance & safety

Aligns with Kubernetes design patterns

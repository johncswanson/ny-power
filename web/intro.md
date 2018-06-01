# Learn how to run non-cloud-native applications on Kubernetes with Helm

Kubernetes is a great environment for running containerized
applications. There are all kinds of best practices and patterns for
creating cloud-native applications for the platform. But sometimes
you need to integrate existing, established services into this
environment. What does the pattern look like for that?

I decided to explore this by creating a project that publishes a real-time data feed via MQTT. MQTT is an open-standard publish / subscribe
protocol that is used extensively in the Internet of Things space. The
ny-power application consumes the regularly published data from the
New York power grid and publishes it using an MQTT service. It also computes the real-time rate of CO2 per kWh usage in the grid. The most commonly used
open-source MQTT service is mosquitto, which is not cloud native, and
requires persistent storage to provide message guarantees. The entire
application ends up being a medium-complexity Kubernetes app, which
demonstrates how legacy services can be used with persistent volumes
alongside more cloud-native or custom-written components.

MQTT also can be consumed directly over websockets. To demonstrate this, an included web dashboard enables you to visualize the data. This means you need to get the external IP address of the MQTT pod that's handed to the
web site pods before they start. You can do this with the
Kubernetes API, but only after you explicitly allow those API calls
using Roles and RoleAssignments. This is done with a ServiceAccount, which avoids  granting too many permissions as a security best practice.

The entire application is wrapped up in Helm, which really shows its
strength once you move beyond trivial applications in Kubernetes. The
ny-power application also demonstrates that your application doesn't
have to be entirely cloud native to benefit from being deployed in
Kubernetes. The built-in service monitoring and restart in Kubernetes,
combined with the lifecycle management of Helm, make this application much
easier to maintain than if it had been done as a traditional set of
co-located services on a Linux server.

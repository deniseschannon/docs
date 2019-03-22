---
title: Projects and Namespaces
weight: 2032
aliases:
  - /rancher/v2.x/en/concepts/projects/
  - /rancher/v2.x/en/tasks/projects/
  - /rancher/v2.x/en/tasks/projects/create-project/
  - /rancher/v2.x/en/k8s-in-rancher/projects-and-namespaces/
---

## Projects

_Projects_ are organizational objects introduced in Rancher that ease the administrative burden of your cluster. You can use projects to support multi-tenancy.

Projects provide an extra level of organization in your Kubernetes clusters beyond [namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/). In terms of hierarchy:

- Clusters contain projects.
- Projects contain namespaces.

Within Rancher, projects allow you manage multiple namespaces as a single entity. In the base version of Kubernetes, which does not include projects, features like role-based access rights or cluster resources are assigned to individual namespaces. In clusters where multiple namespaces require the same set of access rights, assigning these rights to each individual namespace can become tedious. Even though all namespaces require the same rights, there's no way to apply those rights to all of your namespaces in a single action. You'd have to repetitively assign these rights to each namespace!

Rancher projects resolve this issue by allowing you to apply resources and access rights at the project level. Each namespace in the project then inherits these resources and policies, so you only have to assign them to the project once, rather than assigning them to each namespace.

You can use projects to perform actions like:

- Assign users access to a group of namespaces (i.e., [project membership]({{< baseurl >}}/rancher/v2.x/en/k8s-in-rancher/projects-and-namespaces/project-members)).
- Assign users specific roles in a project. A role can be owner, member, read-only, or [custom]({{< baseurl >}}/rancher/v2.x/en/admin-settings/rbac/default-custom-roles/).
- Assign resources to the project.
- Assign Pod Security Policies.


When you create a cluster, two project are automatically created within it:

- [Default Project](#default-project)
- [System Project](#system-project)


### Default Project

When you provision a cluster, it automatically creates a `default` project for the cluster. This is a project you can use to get started with your cluster, but you can always delete it and replace it with projects that have more descriptive names.

### System Project

_Available as of v2.0.7_

When troubleshooting, you can view the `system` project to check if important namespaces in the Kubernetes system are working properly. This easily accessible project saves you from troubleshooting individual system namespace containers.

To open it, open the **Global** menu, and then select the `system` project for your cluster.

The `system` project:

- Is automatically created when you provision a cluster.
- Lists all namespaces that exist in `v3/settings/system-namespaces`, if they exist.
- Allows you to add more namespaces or move its namespaces to other projects.
- Cannot be deleted because it's required for cluster operations.

>**Note:** In clusters where both:
>
> - The [Canal network plug-in]({{< baseurl >}}/rancher/v2.x/en/cluster-provisioning/rke-clusters/options/#canal) is in use.
> - The Project Network Isolation option is enabled.
>
>The `system` project overrides the Project Network Isolation option so that it can communicate with other projects, collect logs, and check health.

### Authorization

Non-administrative users are only authorized for project access after an administrator, cluster owner or cluster member explicitly adds them to the project's **Members** tab.

>**Exception:**
> Non-administrative users can access projects that they create themselves.

### Pod Security Policies

Rancher extends Kubernetes to allow the application of [Pod Security Policies](https://kubernetes.io/docs/concepts/policy/pod-security-policy/) at the project level in addition to the cluster level. However, as a best practice, we recommend applying Pod Security Policies at the cluster level.

### Creating Projects

1. From the **Global** view, choose **Clusters** from the main menu. From the **Clusters** page, open the cluster from which you want to create a project.

1. From the main menu, choose **Projects/Namespaces**. Then click **Add Project**.

1. Enter a **Project Name**.

1. **Optional:** Select a **Pod Security Policy**. Assigning a PSP to a project will:

    - Override the cluster's default PSP.
    - Apply the PSP to the project.
    - Apply the PSP to any namespaces you add to the project later.

    >**Note:** This option is only available if you've already created a Pod Security Policy. For instruction, see [Creating Pod Security Policies]({{< baseurl >}}/rancher/v2.x/en/admin-settings/pod-security-policies/).

1. **Recommended:** Add project members.

    Use the **Members** section to provide other users with project access and roles.

    By default, your user is added as the project `Owner`.

    1. Click **Add Member**.

    1. From the **Name** combo box, search for a user or group that you want to assign project access.

        >**Note:** You can only search for groups if external authentication is enabled.

    1. From the **Role** drop-down, choose a role.

        [What are Roles?]({{< baseurl >}}/rancher/v2.x/en/admin-settings/rbac/cluster-project-roles/)

        >**Notes:**
        >
        >- Users assigned the `Owner` or `Member` role for a project automatically inherit the `namespace creation` role. However, this role is a [Kubernetes ClusterRole](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-and-clusterrole), meaning its scope extends to all projects in the cluster. Therefore, users explicitly assigned the `Owner` or `Member` role for a project can create namespaces in other projects they're assigned to, even with only the `Read Only` role assigned.
        >
        >- Choose `Custom` to create a custom role on the fly: [Custom Project Roles]({{< baseurl >}}/rancher/v2.x/en/admin-settings/rbac/cluster-project-roles/#custom-project-roles).

    1. To add more members, repeat substeps a—c.

1. **Optional:** Add **Resource Quotas**, which limit the resources that a project (and its namespaces) can consume. For more information, see [Resource Quotas]({{< baseurl >}}/rancher/v2.x/en/k8s-in-rancher/projects-and-namespaces/resource-quotas).

    >**Note:** This option is available as of v2.1.0.

    1. Click **Add Quota**.

    1. Select a [Resource Type]({{< baseurl >}}/rancher/v2.x/en/k8s-in-rancher/projects-and-namespaces/resource-quotas/#resource-quota-types).

    1. Enter values for the **Project Limit** and the **Namespace Default Limit**.

        | Field                   | Description                                                                                              |
        | ----------------------- | -------------------------------------------------------------------------------------------------------- |
        | Project Limit           | The overall resource limit for the project.                                                              |
        | Namespace Default Limit | The default resource limit available for each namespace. This limit is propagated to each namespace in the project. The combined limit of all project namespaces shouldn't exceed the project limit.  |

    1. **Optional:** Repeat these substeps to add more quotas.

1. **Optional:** Specify **Container Default Resource Limit**, which will be applied to every container started in the project. The parameter is recommended if you have CPU or Memory limits set by the Resource Quota. It can be overridden on per an individual namespace or a container level. For more information, see [Container Default Resource Limit]({{< baseurl >}}/rancher/v2.x/en/k8s-in-rancher/projects-and-namespaces/resource-quotas/#setting-container-default-resource-limit)
	>**Note:** This option is available as of v2.2.0.


1. Click **Create**.

**Result:** Your project is created. You can view it from the cluster's **Projects/Namespaces** view.

## Switching between Clusters/Projects

To switch between clusters and projects, use the **Global** drop-down available in the main menu.

![Global Menu]({{< baseurl >}}/img/rancher/global-menu.png)

Alternatively, you can switch between projects and clusters using the main menu.

- To switch between clusters, open the **Global** view and select **Clusters** from the main menu. Then open a cluster.
- To switch between projects, open a cluster, and then select **Projects/Namespaces** from the main menu. Select the link for the project that you want to open.

## Namespaces

Within Rancher, you can further divide projects into different [namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/), which are virtual clusters within a project backed by a physical cluster. Should you require another level of organization beyond projects and the `default` namespace, you can use multiple namespaces to isolate applications and resources.

Although you assign resources at the project level so that each namespace can in the project can use them, you can override this inheritance by assigning resources explicitly to a namespace.

Resources that you can assign directly to namespaces include:

- [Workloads]({{< baseurl >}}/rancher/v2.x/en/k8s-in-rancher/workloads/)
- [Load Balancers/Ingress]({{< baseurl >}}/rancher/v2.x/en/k8s-in-rancher/load-balancers-and-ingress/)
- [Service Discovery Records]({{< baseurl >}}/rancher/v2.x/en/k8s-in-rancher/service-discovery/)
- [Persistent Volume Claims]({{< baseurl >}}/rancher/v2.x/en/k8s-in-rancher/volumes-and-storage/persistent-volume-claims/)
- [Certificates]({{< baseurl >}}/rancher/v2.x/en/k8s-in-rancher/certificates/)
- [ConfigMaps]({{< baseurl >}}/rancher/v2.x/en/k8s-in-rancher/configmaps/)
- [Registries]({{< baseurl >}}/rancher/v2.x/en/k8s-in-rancher/registries/)
- [Secrets]({{< baseurl >}}/rancher/v2.x/en/k8s-in-rancher/secrets/)

>**Note:** Although you can assign role-based access to namespaces in the base version of Kubernetes, you cannot assign roles to namespaces in Rancher. Instead, assign role-based access at the project level.

For more information, see [Namespaces]({{< baseurl >}}/rancher/v2.x/en/project-admin/namespaces/).

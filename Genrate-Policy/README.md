# Kyverno Generate Policy – Production Guide

## 1. What is a Generate Policy?

A **Generate Policy** in Kyverno is used to **automatically create Kubernetes resources** when a matching resource is created or updated.

Instead of only validating or mutating existing manifests, generate policies allow Kyverno to **create additional dependent resources automatically**.

### Simple Definition

> A generate policy auto-creates required Kubernetes resources so teams don’t have to create them manually.

---

## 2. Why Generate Policies Are Important

In production Kubernetes clusters:

* Platform teams want **standard behavior everywhere**
* Developers should not manually create supporting resources
* Missing security resources cause vulnerabilities

Generate policies solve this by **automating platform defaults**.

---

## 3. Where Generate Policies Run

Generate policies work in two ways:

1. **Admission Time**

   * When a new resource is created
   * Kyverno immediately generates required resources

2. **Background Mode**

   * Kyverno scans existing resources
   * Generates missing dependent resources

This ensures **new and existing workloads stay compliant**.

---

## 4. How Generate Policies Work (High-Level Flow)

1. A resource is created (for example, a Namespace)
2. Kyverno checks generate rules
3. Kyverno creates one or more Kubernetes resources
4. Generated resources are owned and tracked by Kyverno

Kyverno continuously reconciles generated resources.

---

## 5. Common Production Use Cases

### 5.1 Auto-Generate NetworkPolicies (MOST COMMON)

**Problem:**

* Developers forget to create NetworkPolicies
* No network isolation by default

**Generate Policy Solution:**

* Automatically create a default deny NetworkPolicy for every new namespace

---

### 5.2 Auto-Create ResourceQuotas and LimitRanges

**Problem:**

* Namespaces without quotas cause resource abuse

**Generate Policy Solution:**

* Automatically generate ResourceQuota and LimitRange per namespace

---

### 5.3 Auto-Generate ConfigMaps or Secrets

**Problem:**

* Required configs missing

**Generate Policy Solution:**

* Automatically create standard ConfigMaps or Secrets

---

### 5.4 Security Baseline Enforcement

**Problem:**

* Security objects not created consistently

**Generate Policy Solution:**

* Auto-create PodSecurity policies, network rules, or monitoring configs

---

## 6. Generate Policy vs Mutate vs Validate

| Feature           | Validate     | Mutate   | Generate       |
| ----------------- | ------------ | -------- | -------------- |
| Purpose           | Allow / Deny | Auto-fix | Auto-create    |
| Modifies resource | ❌ No         | ✅ Yes    | ➕ New resource |
| Blocks request    | ✅ Yes        | ❌ No     | ❌ No           |
| Production usage  | 🔥 Very high | High     | High           |

---

## 7. Production Best Practices

1. **Use generate for platform resources**

   * NetworkPolicies
   * ResourceQuotas
   * LimitRanges

2. **Combine with validation**

   * Generate defaults
   * Validate critical requirements

3. **Enable background mode**

   * Ensures existing namespaces comply

4. **Keep generated resources minimal**

   * Avoid over-automation

---

## 8. Real Production Scenario

### Scenario

* EKS cluster with multiple teams
* Namespaces created frequently

### Generate Policies Used

* Default deny NetworkPolicy
* ResourceQuota
* LimitRange

### Outcome

* Secure-by-default namespaces
* No manual intervention
* Consistent cluster governance

---

## 9. When NOT to Use Generate Policies

Avoid generate policies when:

* Resource content must be customized by developers
* Business logic is application-specific
* Generated resources may conflict with app behavior

---


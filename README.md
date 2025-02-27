# Proposal: Developing Xeon Phi 5110P Support for AlmaLinux 9 (Kernel 5.14)

**Introduction**

This proposal outlines an initiative to fully support the Intel Xeon Phi 5110P coprocessor on AlmaLinux 9 by targeting kernel version 5.14. Historically, community patches—most notably for kernel 5.4—have enabled limited functionality of the Xeon Phi 5110P by bridging compatibility gaps, especially in key areas like the Symmetric Communication Interface (SCIF). However, AlmaLinux 9’s kernel 5.14 introduces substantial changes to driver APIs, system interfaces, and network management (with deprecated tools like network-scripts), necessitating a fresh, comprehensive solution. Our goal is to develop a modern, community-maintained kernel module (mic.ko) and update the associated user-space utilities, thereby ensuring that legacy Xeon Phi hardware remains a viable asset for high-performance computing tasks such as AI inference, parallel processing, and scientific simulations.

**Background and Justification**

The Intel Xeon Phi 5110P, built on Intel’s Many Integrated Core (MIC) architecture, was originally designed to offer high parallel processing capabilities at an affordable price point. Despite its discontinuation by Intel, the hardware still holds significant potential for academic and research applications—particularly in regions where budget constraints limit access to modern AI infrastructure. Early community efforts have produced patches for older kernels (e.g., kernel 5.4) that restore basic functionality, but these solutions do not translate directly to the newer, more secure, and performance-optimized environment provided by AlmaLinux 9’s kernel 5.14. 

Key challenges include the need for extensive rework of the SCIF drivers to align with updated kernel internals, overhauling PCIe device handling routines, and replacing deprecated network configuration tools with modern alternatives like NetworkManager. Addressing these issues is critical to ensure that the Xeon Phi 5110P can continue to deliver its exceptional parallel processing capabilities. Moreover, by revitalizing support for this legacy hardware, the project will empower educational institutions with a cost-effective computing solution, enhancing research opportunities in AI and scientific computing without the need for substantial financial investment.

### Discontinued Hardware with Untapped Potential
The **Intel Xeon Phi 5110P**, part of Intel’s now-discontinued **Many Integrated Core (MIC)** architecture, provided exceptional parallel processing power at relatively low cost. However, official support for this hardware ended years ago, and **modern distributions like AlmaLinux 9 (kernel 5.14)** no longer ship compatible drivers or the necessary **Manycore Platform Software Stack (MPSS)**.

### Educational Institutions Need Affordable AI Power
In regions such as **Lima, Peru**, educational institutions often face **severe budget constraints**, limiting their access to modern **AI-capable infrastructure**. By reviving support for the Xeon Phi 5110P on contemporary Linux platforms, this project offers a **cost-effective path** to deploy AI tools for **personalized learning**, **automated grading**, and **intelligent tutoring systems**.

---

## Target Operating System and Kernel Versions

This project will **target the following environment:**

| Distribution | Kernel Version | Status |
|---|---|---|
| AlmaLinux 9 | 5.14.x | Primary Target |
| Rocky Linux 9 | 5.14.x | Secondary Target (Compatibility) |
| RHEL 9 (upstream reference) | 5.14.x | Reference Only |

The project will follow **AlmaLinux 9’s lifecycle**, ensuring long-term compatibility until **2032**.

---

## Objectives

### 1. Develop Modern Kernel Module (`mic.ko`)
- Analyze the **MPSS 3.8.6 kernel module** (last working on CentOS 7/8 with kernel 3.10 and 4.18).
- Refactor and adapt to support **kernel 5.14** and **modern device management interfaces**.
- Package as an **RPM for easy deployment** on AlmaLinux 9.

### 2. Update Essential User-Space Utilities
- Port and test the following MPSS components for **EL9**:
    - `libmicmgmt`
    - `libscif`
    - `mpss-daemon`
    - `micctrl`, `micinfo`, `miccheck`
- Replace deprecated libraries or features (e.g., Python 2 dependencies).

### 3. Publish Complete Documentation
- Step-by-step installation and configuration guides.
- Explanation of **kernel module compatibility challenges** and solutions.
- Practical case studies for **AI inference**, **matrix computation**, or **parallel model training** on Xeon Phi.

---

## Why Focus on AlmaLinux 9?

✅ **RHEL-Compatible**: Direct compatibility with industry-standard RHEL clones, including **Rocky Linux** and **Oracle Linux**.  
✅ **Long-Term Support**: Guaranteed updates until **2032**.  
✅ **Community Driven**: Ideal platform for educational and research use.  
✅ **Modern Kernel and Toolchain**: Ships with **kernel 5.14**, **GCC 11**, **glibc 2.34**, and **systemd 249** — all requiring compatibility work for legacy MPSS components.

---

## Strategic Impact

### For Educational Institutions
- **Low-Cost AI Infrastructure**: Repurpose affordable or donated Xeon Phi hardware for AI workloads.
- **Modern Security**: Run AI applications on a supported, patched OS with current security practices.
- **Learning Platform**: Provides real-world case studies for **open hardware integration** into education.

### For Open-Source Community
- **Rescue Legacy Hardware**: Preserves the value of Xeon Phi hardware, aligning with **open hardware reuse** principles.
- **Kernel Adaptation Blueprint**: Serves as a case study for adapting out-of-tree drivers to modern kernels.
- **Collaborative Development**: Invites contributions from kernel developers, hardware enthusiasts, and educational technologists.

---

## Technical Challenges

| Challenge | Description | Proposed Solution |
|---|---|---|
| Kernel API Changes | Many interfaces changed between **3.10 → 5.14** | Review upstream kernel commits and rewrite affected code. |
| Systemd Integration | MPSS expects legacy init system | Convert all management to **systemd units**. |
| Library Compatibility | Deprecated dependencies (Python 2, old glibc) | Patch for Python 3, rebuild with **glibc 2.34**. |
| Device Enumeration | Kernel’s PCI device handling evolved | Use **modern PCI enumeration APIs**. |

---

## Project Timeline (Tentative)

| Phase | Duration | Deliverable |
|---|---|---|
| Initial Analysis | 4 weeks | Compatibility Report |
| Kernel Module Porting | 8 weeks | mic.ko for 5.14 |
| User-Space Porting | 6 weeks | Updated MPSS Utilities |
| Packaging & Documentation | 4 weeks | RPMs + Full Docs |
| Testing & Validation | 4 weeks | Real-World AI Workload Tests |
| Community Outreach | Ongoing | Blog, Talks, Collaboration |

---

## License

- All future code contributions will follow the **GPLv2** license (matching kernel requirements).
- Documentation and educational materials will be published under **Creative Commons Attribution 4.0 (CC BY 4.0)**.

---

## Call to Action: Contributors Wanted!

This project seeks **kernel developers**, **hardware enthusiasts**, **educational technologists**, and **AI researchers** interested in:

✅ Open hardware support for legacy accelerators.  
✅ Affordable AI infrastructure for education.  
✅ Real-world kernel driver development for EL9.  
✅ Collaboration on AI tools tailored for **basic education in Peru** and beyond.

---

## References

- Intel MPSS Documentation (Archived)
- Keijser, J.J. (2024). Adapting MPSS for CentOS 8. GitHub Repository: [https://github.com/jjkeijser/mpss](https://github.com/jjkeijser/mpss)
- AlmaLinux 9 Release Notes: [https://wiki.almalinux.org/release-notes/9.0.html](https://wiki.almalinux.org/release-notes/9.0.html)

---

## Repository Structure (Planned)

```plaintext
/
├── kernel-module/
│   ├── mic/
│   ├── build-scripts/
│   └── patches/
├── user-space/
│   ├── libmicmgmt/
│   ├── libscif/
│   ├── mpss-daemon/
│   └── tools/
├── docs/
│   ├── installation.md
│   ├── configuration.md
│   ├── compatibility.md
│   └── case-studies/
├── packaging/
│   ├── RPM SPECS/
│   ├── SRPM/
│   └── repos/
└── README.md  (this file)
```

---

**Together, let’s bring Xeon Phi back to life — for education, for AI, and for open hardware innovation.**

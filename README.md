# ClarityRS

**ClarityRS** is a **secure Rust-based backend and management system** for running Windows applications in fully isolated virtual machines on Linux. It orchestrates Windows 11 VMs with GPU acceleration and exposes a **programmatic API** for seamless integration and automation. ClarityRS integrates Windows applications into the Linux desktop using **Looking Glass**, optionally enhanced with **Gamescope** for high-performance display streaming.

---

## **Key Features**

* **Secure Windows Containers / VMs**
  Fully isolated Windows 11 environments with minimal attack surface. Supports kernel-level drivers inside VMs, including anticheat.

* **Looking Glass Integration**
  Low-latency framebuffer capture and input forwarding to run Windows apps seamlessly on Linux.

* **GPU Acceleration / vGPU Support**
  NVIDIA GPU passthrough or virtual GPU slices for high-performance graphics and smooth app rendering.

* **Linux Desktop Integration**
  Windows apps appear as native Linux windows with real-time input and display support.

* **Extensible API**
  Programmatic access to VM and app lifecycle, GPU assignment, resource management, and streaming sessions.

* **Tauri Frontend (Optional)**
  Lightweight, secure, and cross-platform UI for managing Windows VMs and apps without leaving Linux.

* **Gamescope Support (Optional)**
  GPU-accelerated micro-compositor to simplify window rendering, scaling, and input forwarding for Windows apps.

---

## **Architecture Overview**

1. **Rust Backend**

   * Orchestrates VMs and containers (Docker/Podman)
   * Manages GPU assignment, Looking Glass sessions, and VM resources
   * Exposes REST API for frontend or automation scripts

2. **Windows VM Layer**

   * Full Windows 11 kernel with pre-installed apps and optional kernel drivers
   * Runs inside a lightweight utility VM or container wrapper

3. **Looking Glass / Gamescope**

   * Captures framebuffer from VM
   * Streams display to Linux host
   * Handles input forwarding and optional GPU-based rendering

4. **Frontend (Tauri)**

   * Secure GUI for launching apps, monitoring VMs, and configuring resources

---

## **Example Usage**

**Launch a Windows app via API:**

```http
POST /v1/apps/launch
{
    "vm_id": "win11-game1",
    "app_path": "C:\\Program Files\\Game\\game.exe",
    "gpu": true
}
```

* Backend ensures VM is running.
* Starts the app inside the VM.
* Initializes Looking Glass framebuffer streaming.
* Optionally runs Gamescope for GPU-accelerated rendering.
* Returns session handle for frontend integration.

---

## **Getting Started**

1. **Install dependencies:**

   * QEMU/KVM
   * NVIDIA drivers (if GPU passthrough is required)
   * Rust toolchain
   * Looking Glass (guest and host modules)
   * Optional: Gamescope for display compositing

2. **Build ClarityRS backend:**

```bash
cargo build --release
```

3. **Configure VM templates and GPU:**

* Create or use a Windows 11 VM image (pre-installed apps, optional drivers)
* Configure GPU passthrough or vGPU slices

4. **Run backend:**

```bash
./target/release/clarityrs
```

5. **Use Tauri frontend (optional):**

* Launch Windows apps, monitor sessions, manage resources

---

## **Security Considerations**

* All kernel-level drivers are loaded **inside Windows VMs only** — Linux host remains isolated.
* API access requires authentication tokens or TLS certificates.
* GPU and memory resources are isolated per VM.
* Logs and snapshots provide audit trails for secure operation.

---

## **Planned Features**

* Per-app VM snapshots and rollback
* Automated GPU allocation for multiple VMs
* Enhanced Linux desktop embedding (Wayland/X11 integration)
* Advanced resource scheduling and monitoring
* CI/CD for VM image updates and driver deployment

---

## **License**

[MIT License](LICENSE)

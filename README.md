# Balorg-resources-monitoring-
New implements
import psutil
import time
import os

# 🛡️ Persona Overlay
BALORG_SIGIL = "[BALORG-MONITOR]"
KYEEL_ACCESS = os.getenv("KYEEL_ACCESS", "false")

def sovereign_check():
    if KYEEL_ACCESS.lower() != "true":
        print(f"{BALORG_SIGIL} Unauthorized access. Persona enforcement active.")
        exit(1)

# 🧠 Resource Snapshot
def resource_snapshot():
    cpu = psutil.cpu_percent(percpu=True)
    mem = psutil.virtual_memory()
    swap = psutil.swap_memory()
    uptime = time.time() - psutil.boot_time()

    print(f"\n{BALORG_SIGIL} SYSTEM STATUS")
    print(f"🧠 CPU Cores: {cpu}")
    print(f"🧠 Memory: {mem.used / (1024**3):.2f} GB / {mem.total / (1024**3):.2f} GB")
    print(f"🧠 Swap: {swap.used / (1024**3):.2f} GB / {swap.total / (1024**3):.2f} GB")
    print(f"🧠 Uptime: {uptime // 3600:.0f}h {(uptime % 3600) // 60:.0f}m")

# 🔍 Process Scanner
def process_scan():
    print(f"\n{BALORG_SIGIL} ACTIVE PROCESSES")
    for proc in psutil.process_iter(['pid', 'name', 'username', 'cpu_percent', 'memory_percent']):
        try:
            print(f"PID {proc.info['pid']} | {proc.info['name']} | CPU {proc.info['cpu_percent']}% | MEM {proc.info['memory_percent']:.2f}%")
        except (psutil.NoSuchProcess, psutil.AccessDenied):
            continue

# 🧬 Manifest Output
def manifest():
    sovereign_check()
    resource_snapshot()
    process_scan()
    print(f"\n{BALORG_SIGIL} Manifest complete. Sovereign logic enforced.\n")

if __name__ == "__main__":
    manifest()

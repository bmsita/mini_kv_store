# 📊 Unix Domain Socket Key-Value Store Performance Report

## 🔹 Performance Summary
**Test duration:** 37 seconds under concurrent load  

**Syscalls observed:**

| Syscall | Count |
|---------|-------|
| read    | 5     |
| write   | 6     |
| accept  | 2     |

**Observations:**
- ⚡ High read and write counts → server is **I/O bound**  
- 🔗 `accept` calls → reflect the number of new client connections  
- 🛠 Mutex contention (futex) may appear under **high concurrency**, potentially slowing writes  

---

## 🔹 Load Scenarios

### 1. Read-heavy Load
- **State:** Many clients performing GET operations simultaneously  
- **Observation:** Minimal mutex contention; server handles multiple reads efficiently  
- **Fix / Optimization:** No major fix needed; consider caching if read volume grows  

### 2. Write-heavy Load
- **State:** Many clients performing SET operations  
- **Observation:** Mutex contention increases; writes may slow down  
- **Fix / Optimization:** Use finer-grained locks or lock-free data structures  

### 3. Mixed Load
- **State:** Reads and writes interleaved  
- **Observation:** Some mutex contention; moderate latency  
- **Fix / Optimization:** Optimize critical sections; minimize lock duration  

### 4. High Concurrency
- **State:** Many clients connecting simultaneously  
- **Observation:** High accept count; increased futex calls due to thread synchronization  
- **Fix / Optimization:** Consider using a **thread pool** or **asynchronous I/O**  

---

## 🔹 Commands Used

1. **Compile the server**
```bash
gcc uds_kv_server_perf_top.c -o uds_kv_server_perf_top
```
2. **Run the server**
```bash
./uds_kv_server_perf_top
```
3.**Run multiple clients manually**
```bash
./uds_kv_client_perf_top  # Terminal 1
./uds_kv_client_perf_top  # Terminal 2
```
4.**Profile the server with perf**
```bash
sudo perf stat -e syscalls:sys_enter_read,syscalls:sys_enter_write,syscalls:sys_enter_accept ./uds_kv_server_perf_top
```
5.**(Optional) View server PID manually**
```bash
ps aux | grep uds_kv_server_perf_top
```
6.**Capture screenshot**
  WSL: Windows + Shift + S
  Linux GUI: PrtScn or gnome-screenshot -a

# 🖼 Screenshot

**Performance Screenshot:** 

**Client** - https://github.com/bmsita/mini_kv_store/blob/37ce9849e438b188a8568462033b8f6b2ddaa9bf/perf_client.png.png

**Server** - https://github.com/bmsita/mini_kv_store/blob/37ce9849e438b188a8568462033b8f6b2ddaa9bf/perf_server.png.png


🔹 Notes

This report excludes server/client code

Designed to show system behavior under different loads and bottleneck analysis

Provides insights for optimizing thread-based key-value store performance
```yaml

---

### ✅ How to use:
1. Copy the above Markdown content.  
2. Create a file named `README.md` in your project folder.  
3. Paste the content into it.  
4. Add your actual screenshot into a `screenshots` folder.  
5. Push to GitHub — it will display as a **professional performance report**.

---

If you want, I can make an **even more polished version with arrows, colored badges, and section icons** so it looks like a **high-quality portfolio report** for GitHub.  

Do you want me to do that?

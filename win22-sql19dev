Thanks for sharing your `web.config`. Based on the error you’re getting:

> **HTTP Error 500.19 – Error Code 0x80070021**  
> **Config Error:** This configuration section cannot be used at this path.

That usually means some section (like `<handlers>`, `<modules>`, or `<httpHandlers>`) is **locked at a parent level** (likely in the root `applicationHost.config` or a higher-level `web.config`).

---

### 🔍 Key Problem Area (Based on the error)

The error points to this part:

```xml
<handlers>
  <remove name="ExtensionlessUrlHandler-Integrated-4.0" />
```

This means **IIS is not allowing overrides of the `handlers` section** at this level.

---

### ✅ Fix: Unlock `handlers` section

You need to **unlock the `handlers` section** from the IIS root config.

You can do this in one of two ways:

#### 🛠 Option 1: Use Command Line (recommended)

Open **Command Prompt as Administrator**, and run:

```bash
%windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers
```

Do the same for `modules`, just in case:

```bash
%windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/modules
```

This allows the `<handlers>` and `<modules>` sections to be used in site-level `web.config`.

---

#### 🧰 Option 2: IIS Manager GUI

1. Open **IIS Manager**
2. Click the **server node** (not the website)
3. Double-click **"Configuration Editor"**
4. In the top dropdown, navigate to: `system.webServer/handlers`
5. On the right-side panel, click **"Unlock Section"**
6. Repeat for `system.webServer/modules`

---

### 🔄 After Unlocking

- **Restart IIS** (use `iisreset` in admin CMD)
- **Reload your site** and the error should go away

---

### 📝 Extra Notes

- This often happens when the parent IIS config (`applicationHost.config`) has `<section name="handlers" overrideModeDefault="Deny" />`
- If you're on **shared hosting**, you might not be able to unlock — you’d need to remove that section or ask the host to unlock it
- Your `web.config` seems like it's built for **Integrated Pipeline Mode**, so make sure your app pool is using that

---

Want me to check if any other section might cause similar issues?



You're getting an HTTP Error 500.21, which usually means your IIS setup can't correctly handle managed code requests—typically because ASP.NET is either not installed, not properly registered, or your IIS configuration is missing something.

The key error:
> **Handler "ExtensionlessUrlHandler-Integrated-4.0" has a bad module "ManagedPipelineHandler"**

This means that your system is trying to use the **.NET Framework 4.0** handler for ASP.NET requests, but IIS doesn't know how to process it—probably because the pipeline isn't properly connected to .NET yet.

---

### ✅ Steps to Fix It

#### 1. **Re-register ASP.NET with IIS**
Run the **ASP.NET IIS Registration Tool**. Open **Command Prompt as Administrator** and run:

```cmd
%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet_regiis.exe -i
```

If you're on a 64-bit system and your app pool is using 64-bit:

```cmd
%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet_regiis.exe -i
```

This re-registers ASP.NET 4.0 with IIS.

---

#### 2. **Enable Required IIS Features**
Make sure that the following features are installed:

1. Open **"Turn Windows features on or off"** (Search it in Start Menu).
2. Ensure these are **checked**:
   - **.NET Framework 4.x**
   - **ASP.NET 4.x**
   - **Internet Information Services**
     - **Web Management Tools**
     - **World Wide Web Services**
       - **Application Development Features**
         - ASP.NET 4.x
         - .NET Extensibility 4.x
         - ISAPI Extensions
         - ISAPI Filters
       - **Common HTTP Features**
         - Default Document
         - Static Content
         - HTTP Errors
         - HTTP Redirection (if you use it)

Then restart your machine (or at least IIS).

---

#### 3. **Restart IIS**
After registration and enabling features, restart IIS:

```cmd
iisreset
```

---

#### 4. **Check Your Web.config**
Sometimes the handler entries in `web.config` are wrong. Ensure you're not manually referencing a bad handler module unless you know what you're doing. If you've customized it, you can paste the `<system.webServer>` section here, and I’ll help validate it.

---

Let me know if you’re running .NET Core or .NET 5/6+, or if this is an older ASP.NET WebForms/MVC app—it can change the solution slightly.

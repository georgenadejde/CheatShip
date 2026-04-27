# Frida

Dynamic instrumentation — Android/iOS/Windows function hooking, root bypass, secret extraction.

---

## Setup — Android

```bash
# 1. download frida-server for device arch
#    https://github.com/frida/frida/releases

# 2. push to device
adb push frida-server /data/local/tmp/

# 3. get root shell and start server
adb shell
su
cd /data/local/tmp
chmod +x frida-server
./frida-server &

# 4. install target APK
adb install target.apk

# 5. verify connection
frida-ps -Uai
```

!!! warning
    If "Failed to enumerate processes: unable to handle 64-bit processes" -> you have the wrong architecture frida-server. Get `arm64` for modern phones.

---

## Attaching to Apps

```bash
# list running apps
frida-ps -Uai

# spawn fresh (recommended)
frida -U -f com.example.app

# resume after spawn
%resume

# spawn + immediately load script
frida -U -f com.example.app -l script.js

# attach to already running app
frida -U -n "App Name" -l script.js

# reload script without restarting
%reload

# save output to file
frida -U -n "App Name" -l listclasses.js > results.txt
```

---

## Core Scripts

### List all app classes

```javascript
Java.perform(() => {
  Java.enumerateLoadedClasses({
    onMatch: function(name, handle) {
      if (name.includes("com.example.app")) {
        console.log(name);
      }
    },
    onComplete: function() { console.log("done"); }
  });
});
```

### List methods of a class

```javascript
Java.perform(() => {
  const cls = Java.use("com.example.app.MainActivity");
  const props = Object.getOwnPropertyNames(cls);
  console.log(props.join("\n"));
});
```

---

## Hooking Functions

### Basic hook — override return value

```javascript
Java.perform(() => {
  const cls = Java.use("com.example.app.CheckClass");

  // force return false
  cls.isRooted.implementation = function() {
    console.log("[*] hooked isRooted");
    return false;
  };

  // hook overloaded method (with parameters)
  cls.check.overload("java.lang.String").implementation = function(s) {
    console.log("[*] param:", s.toString());
    return true;   // bypass check
  };
});
```

### Root bypass — OWASP UnCrackable Level 1

```javascript
Java.perform(() => {
  const c = Java.use("sg.vantagepoint.a.c");
  c.a.implementation = function() { return false; };
  c.b.implementation = function() { return false; };
  c.c.implementation = function() { return false; };
});
```

---

## Extracting Secrets

Hook the decryption function and reconstruct the chain from Jadx source to get the plaintext secret at runtime.

```javascript
Java.perform(() => {
  const base64   = Java.use("android.util.Base64");
  const bClass   = Java.use("sg.vantagepoint.uncrackable1.a");
  const aClass   = Java.use("sg.vantagepoint.a.a");
  const strClass = Java.use("java.lang.String");

  const ciphertext = base64.decode(
    "5UJiFctbmgbDoLXmpL12mkno8HT4Lv8dlat8FxR2GOc=", 0
  );
  const key       = bClass.b("8d127684cbc37c17616d806cf50473cc");
  const decrypted = aClass.a(key, ciphertext);

  console.log("Secret:", strClass.$new(decrypted));
});
```

!!! tip
    Always decompile the APK with **Jadx** first and trace the call chain before writing Frida scripts. Understand what the app is doing before you hook it.

---

## Objection (Frida wrapper)

```bash
# root bypass in one command
objection -g com.example.app explore -s "android root disable"

# if app detects Frida before objection injects:
# terminal 1 — freeze app at spawn
frida -U -f com.example.app
# terminal 2 — inject objection
objection -g com.example.app explore -s "android root disable"
# terminal 1 — resume
%resume

# run a script from objection CLI
%exec script.js
```

---

## Frida on Windows

```javascript
// list modules loaded by a process
Process.enumerateModules().forEach(m => console.log(m.name));

// hook Win32 MessageBoxW — read and replace text
const MessageBoxW = Module.findExportByName("user32.dll", "MessageBoxW");
Interceptor.attach(MessageBoxW, {
  onEnter: function(args) {
    console.log("MessageBox text:", args[1].readUtf16String());
    args[1] = Memory.allocUtf16String("Hooked!");
  }
});
```

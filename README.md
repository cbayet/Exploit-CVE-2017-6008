# Exploit-CVE-2017-6008

The CVE-2017-6008 is a vulnerability in the HitmanPro scan that allows privilege escalation by exploiting a kernel pool buffer overflow.
The exploits here use the Quota Process Pointer Overwrite attack as described in the [Tarjei Mandt's paper](http://www.mista.nu/research/MANDT-kernelpool-PAPER.pdf)

Also, the exploits use my [Pool sprayer library](https://github.com/cbayet/PoolSprayer)

You can find a detailed paper on the Windows 7 exploit [here](https://www.gatewatcher.com/en/news/blog/kernel-pool-overflow-exploitation-in-real-world-windows-7)


# Windows 10 version

This version use another vulnerability in the hitmanpro37.sys driver, an Out-Of-Bounds read, which we use to leak the Pool Cookie.
This leak allows us to use the very same attack on Windows 10.

You can find a detailed paper of the exploit on Windows 10 [here](https://trackwatch.com/kernel-pool-overflow-exploitation-in-real-world-windows-10/)

# Update

The exploit used a technique described by Cesar Cerrudo in 2012 to elevate it's privileges by setting the field Privileges.Enabled of the TOKEN structure.

However, since Windows 10 v1607, the kernel now also checks the value of the Privileges.Present field of the Token.
The Privileges.Present field of the token is the list of privileges that CAN be enabled for this token, by using the AdjustTokenPrivileges API.
So the actual privileges of the TOKEN is now the bitfield resulting of Privileges.Present & Privileges.Enabled.

This means that the exploit in this current state doesn't work anymore since Windows 10 v1607.
Also, the exploit is also broken by the changes that happend in the pool since Windows 19H1, with the arrival of the segment heap in the kernel.

Updated work on Kernel pool exploitation since 19H1 is coming soon !

Reasons for virtualization: 
Run a different OS for testing (Developed on Windows, want to run on Linux)
	Traditional Hypervisors, Type 2
In the cloud, more machines are being run on those servers
Things to think about:
safety: each hypervisor has full control of its resources
Fidelity: Behavior of the program on machine is identical to the behavior on bare hardware
Efficiency: Much of code in vm should run w/o intervention by hypervisor

Type 1 and Type 2
Type 1: Runs on "bare metal".  A thin layer that talks to hardware, OS runs on top of the thin Layer
Type 2: Something like VMware, Virtualbox. Runs on another OS. You have Hardware, an Os on top, the Type 2 hypervisor runs as software, and then you have the guest OS.

Challenges in Bringing Virtualization to the X86
Core attributes of a virtual machine to x86-based target platform:
1. Compatibility
2. Performance
3. Isolation

Major Challenges:
1. x86 was not designed for virtualizability
2. Huge complexity
3. x86 machines had diverse peripherals - you can go out and buy any I/O device and as long as there was a device driver, it was good
4. Need for a simpler user experience

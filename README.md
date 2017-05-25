# IPP-engine-for-ovs
The IPP engine is a programmable data plane engine.
Administrator can inject his own IPP code, a programmed data plane operation procedure, to the IPP engine in run-time.
So a data plane with the IPP engine has run-time programmablility.

We implement a prototype IPP engine based on the Open vSwitch 2.4.0 (OVS). 
The OVS is a well-known SDN data plane, and currently running on plenty of cloud servers to make their network virtualization available.

We recommend you to run our prototype implementation on Ubuntu Desktop 14.04 environment, because it is tested only on the envirionment.

# IPP code written by the IPP language
IPP language inherits most of the features of BSD Packet Filter (BPF), the de-facto software based paceket filtering architecture, 
especially its linux socket filter version (LSF).
You can cunsult the document [1] to know basic language features of BPF. 
We add some additional language features to the BPF (refer to [2]), to support packet processing operations as a SDN data plane such as forwarding and modifying.

# IPP engine components
The IPP engine mainly composed by the loader, the scheduler, the execution engine, and the runtime storage.

The loader is a component to adopt new IPP codes from administrator.
It is implemented as a character device, so the administrator (or an administrating application) can inject his own IPP code to the device by any familiar way.
As an example, the sample IPP code can be installed to the IPP engine like this: "cat /home/wowook/ipp_prog_sample >>/dev/ipploader"
When it receives an IPP code, it converts the code to an executable IPP instance by just-in-time (JIT) compiling. 
The IPP instances are stored in the runtime storage, and called by the scheduler when any events it involved arises.

The scheduler and the execution engine is running on the OVS datapath (the kernel module of OVS as a network device).
The scheduler put tasks to the execution engine. It schedules the executable codes in instances to process a incoming packet.
In the later version, the task is expanded to general event, including timer, user defined, etc.
The execution engine has unit functions for each instructions.
The functions are allow to access non-volatile variables, packet header and data, registers (such as program counter), etc.
The execution engine runs the functions to handle instructions and observe them to prevent the fault executions.












[1] J. Schulist et al, Linux Socket Filtering aka Berkeley Packet Filter (BPF), www.kernel.org/doc/Documentation/networking/ﬁlter.txt.

[2] K. Kiwook et al, "Instruction based Packet Processing Scheme for Programmable SDN Data Plane." JOURNAL OF PLATFORM TECHNOLOGY 4.4 (2016): 71-77.

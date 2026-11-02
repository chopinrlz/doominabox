You are going to create an x86 DOS emulator, with CPU support from the base 8086 processor up to the Pentium processor, with real mode and protected mode operation, implemented on .NET 10 in C#, designed to run on Microsoft Windows 11. The emulator should be designed as a Windows Forms desktop application with a console application as well for running DOS applications directly via command line.

The primary purpose for this emulator is to run the original id software DOOM binary from December 1993.

The Windows Forms desktop application should give the user a GUI where they can open a DOS application to run from their computer. The idea for this emulator is to provide a simple way for a user to run a DOS application in a sandbox environment to enjoy old games or use old programs. Both the Windows Forms application and the console application should use a common backend library which implements the emulator itself. The emulator should be able to emulate all the features of DOS required to execute a standard game or application which includes the file system, memory access, graphics output, and sound.

Here are some features and operating parameters you should consider as you build this software:

1. In-memory file system. When a user runs a DOS application, the directory path on the host machine should be mapped into the `C:\APP` path on the DOS guest environment. Everything that happens on the virtual file system should persist in memory only, nothing should be written back to the host machine's file system. The imaginary disk size of the guest file system should be 768 MiB which is enough to store the contents of an entire CD-ROM with some additional space to spare. Do not allocate all of this space in memory intially; allocate virtual disk space on demand in blocks of 8 MiB as the running application writes to disk.

2. Virtualized memory access. Memory access should be virtualized to allow DOS applications to access standard memory areas common to DOS including conventional memory, high memory, and extended memory. These settings should be tunable via a JSON file which controls how the emulator reports available memory to the guest application because some guest applications do not work properly if too little or too much conventional memory is available.

3. Graphics support. Graphics output should support VGA, SVGA and VESA modes between 320x240 up to 1280x1024 resolution. The Graphics surface itself should use a double-buffered GDI+ graphics object to draw to a window on the host machine.

4. Audio and music. Sound output should emulate the SoundBlaster 16 and Adlib hardware devices, and should also include PC speaker devices. The actual output of the sounds should be routed to the Windows 11 sound system so it can play on the host machine. Sound banks can be simulated rather than rendered from sound fonts or synth packs to ensure that no external dependencies are needed to render sounds or MIDI music.

5. Input devices. Input support should be limited to the keyboard and mouse. Trap the mouse when the use clicks on the graphics window for the emulator and release it if they press `Ctrl + F10`

6. Starting an emulation session. When a user wants to run a DOS application, the emulator should load the raw executable file into memory and start executing the instructions immediately. This will require a byte code emulator similar to what can be found in DOSBOX and DOSBOX-X implementations which are open-source and available on the Internet. However, we do NOT want to wrap this application around DOSBOX, we want our application to be a fully native emulator capable of running DOOM without any external dependencies.

7. Performance. We want the emulator to, hopefully, be about as fast as a 90 MHz Pentium CPU. The CPU parameters should be tunable through the same JSON configuration file that is used to tune the memory settings for the DOS environment itself.
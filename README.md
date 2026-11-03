# DOOM-IN-A-BOX

> _Open-source .NET 10 C# DOS emulator for playing the O.G. id software DOOM from 1993._

# Overview, Purpose & Mission

DOOM-IN-A-BOX is an open-source DOS emulator purpose-built to play the original DOOM binary from December of 1993. It's an experimental project intended to be used to educate software developers on the intricacies of hardware and hardware emulation inside a contained environment. It is also an experiment to test the limits of modern AI coding models to see if they are capable of highly complex tasks like building an x86 emulator that can run software which was compiled decades ago.

Using a managed programming language to build an emulator sacrifices performance in the name of accessibility and portability. Anyone running a Windows PC can fire this up by running a single terminal command, modify the code to tweak performance or functionality to make it their own, and review the code to understand how an emulator works.

Certain portions of the emulation output are black boxed behind platform libraries, like the audio output to Windows, because .NET 10 does not have managed classes (or libraries) for audio output. However, the import statements and library calls are clearly visible in the source code for the emulator itself so you can see where audio is sent for playback.

## Design

The emulator is written in C#, a managed programming langauge, to maximize the ease of understanding and simplicitly of use. It runs on .NET 10, a free run-time, on the Windows operating system. The application is a simple Windows Forms desktop application where you select an EXE file and the program starts immediately. The UX is intended to be simple, with minimal configuration settings.

File system support is emulated in-memory only, meaning when you open an EXE, the emulator maps the root directory of that EXE to the `C:\APP` directory on the guest filesystem in-memory. The guest EXE can read and write to and from its virtual disk, but no changes are saved back to your host so nothing is modified on your actual PC. This means you cannot create new save games, but you can load your old save games.

Because the emulator draws graphics to the screen and renders audio streams from the PC speaker and beyond, the emulator only supports Windows. The emulator itself was built using the Cursor development environment and is comprised of mostly AI-generated code with a number of manual modifications and optimizations.

## Cost

Developing software with AI is not free. The total cost of this application, should you want to build it yourself from scratch using your own AI tools, was approximately USD $150. The entire build took approximately 2 days.

## Building Your Own Emulator

Take a look at the [PLAN.md](PLAN.md) Markdown file for instructions on building your own DOOM emulator. This plan is C# specific because it references .NET elements in the details. You can hand this plan directly to your AI tool of choice to build the emulator from scratch.

# Getting Started

To run DOOM-IN-A-BOX you will only need a few things:

1. A copy of the DOOM shareware -- which you can get from ModDB [https://www.moddb.com/games/doom/downloads/doom-shareware-v19](https://www.moddb.com/games/doom/downloads/doom-shareware-v19) -- or the DOOM registered version if you already own it

2. .NET 10 SDK [https://dotnet.microsoft.com/en-us/download/dotnet/10.0](https://dotnet.microsoft.com/en-us/download/dotnet/10.0)

3. This repo cloned somewhere locally so you can build the Windows Forms application and run it

Put `DOOM.EXE` and `DOOM.WAD` into their own directory somewhere. Start DOOM-IN-A-BOX from your terminal using this command:

```powershell
dotnet run --project .\src\DoomInABox.WinForms\DoomInABox.WinForms.csproj
```

Run that command from the root of the repo, dotnet will compile and start DOOM-IN-A-BOX.

# Sound and Music Support

DOOM relied on a file called `DEFAULT.CFG` which it read at run-time to determine how to address sound and music devices. By default, this emulator uses the following hardware settings for sound and music:

|Setting|Value|
|-|-|
|Address|220|
|IRQ|7|
|DMA Channel|1|
|HDMA Channel|5|

## The DOOMDATA Directory

DOOM, unlike DOOM 2, only reads the `DEFAULT.CFG` file located at the fixed path `C:\DOOMDATA\DEFAULT.CFG` -- it ignores the file from the local path. As such, for sound and music to work, this emulator includes an **Exception** for DOOM, specifically, to map the `DEFAULT.CFG` file found next to the DOOM executable to the `C:\DOOMDATA\DEFAULT.CFG` so it reads the sound settings. There is a toolbar button on the emulator window which turns these exceptions on and off.
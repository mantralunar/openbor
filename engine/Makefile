#
# OpenBOR - https://www.ChronoCrash.com
# ---------------------------------------------------------------------------------------------------
# All rights reserved, see LICENSE in OpenBOR root for details.
#
# Copyright (c) OpenBOR Team
#

#----------------------------------------------------------------------------------------------------
#
#               	OpenBOR Makefile for ALL TARGET_PLATFORMs
#
#----------------------------------------------------------------------------------------------------

ifndef VERSION_NAME
VERSION_NAME = OpenBOR
endif

#----------------------------------------------------------------------------------------------------
# Defines
#----------------------------------------------------------------------------------------------------

ifdef BUILD_WIN
TARGET          = $(VERSION_NAME).elf
TARGET_FINAL    = $(VERSION_NAME).exe
TARGET_PLATFORM = WIN
TARGET_ARCH     = x86
TARGET_RESOURCE = resources/$(VERSION_NAME).res
OBJTYPE         = win32
BUILD_SDL       = 1
BUILD_GFX       = 1
BUILD_STATIC    = 1
BUILD_SDL_IO    = 1
BUILD_OPENGL    = 1
BUILD_LOADGL    = 1
BUILD_VORBIS    = 1
BUILD_WEBM      = 1
BUILDING        = 1
YASM 	        = yasm$(EXTENSION)
CC              = $(WINDEV)/$(PREFIX)gcc$(EXTENSION)
INCLUDES        = $(SDKPATH)/include \
                  $(SDKPATH)/include/SDL2 \
		  $(EXTRA_INCLUDES)
LIBRARIES       = $(SDKPATH)/lib \
		  $(EXTRA_LIBRARIES)
ARCHFLAGS       = -m32
ifeq ($(findstring 86, $(TARGET_ARCH)), 86)
BUILD_MMX       = 1
endif
ifeq ($(BUILD_WIN), 0)
BUILD_DEBUG     = 1
endif
endif


ifdef BUILD_LINUX
TARGET 	        = $(VERSION_NAME).elf
TARGET_FINAL    = $(VERSION_NAME)
TARGET_PLATFORM = LINUX
BUILD_SDL       = 1
BUILD_GFX       = 1
BUILD_PTHREAD   = 1
BUILD_SDL_IO    = 1
BUILD_OPENGL    = 1
BUILD_LOADGL    = 1
BUILD_VORBIS    = 1
BUILD_WEBM      = 1
BUILDING        = 1
YASM 	        = yasm
CC  	        = $(LNXDEV)/$(PREFIX)gcc
OBJTYPE         = elf
INCLUDES        = $(SDKPATH)/include
ifeq ($(findstring 64, $(GCC_TARGET)), 64)
TARGET_ARCH     = amd64
BUILD_AMD64     = 1
ARCHFLAGS       = -m64
LIBRARIES       = $(SDKPATH)/lib/$(GCC_TARGET)
CFLAGS          += -DAMD64
else
TARGET_ARCH     = x86
ARCHFLAGS       = -m32
LIBRARIES       = $(SDKPATH)/lib32
BUILD_MMX       = 1
endif
ifeq ($(BUILD_LINUX), 0)
BUILD_DEBUG     = 1
endif
endif

ifdef BUILD_LINUX_LE_x86_64
TARGET 	        = $(VERSION_NAME).elf
TARGET_FINAL    = $(VERSION_NAME)
TARGET_PLATFORM = LINUX
BUILD_SDL       = 1
BUILD_GFX       = 1
BUILD_PTHREAD   = 1
BUILD_SDL_IO    = 1
BUILD_OPENGL    = 1
BUILD_LOADGL    = 1
BUILD_VORBIS    = 1
BUILD_WEBM      = 1
BUILDING        = 1
YASM 	          = nasm
OBJTYPE         = elf
INCLUDES        = $(SDKPATH)/usr/include \
                  $(SDKPATH)/usr/include/SDL2
TARGET_ARCH     = amd64
BUILD_AMD64     = 1
ARCHFLAGS       = -m64
LIBRARIES       = $(SDKPATH)/usr/lib
CFLAGS          += -DAMD64
# Workaround for GCC 9
CFLAGS          +=  -Wno-error=stringop-overflow
endif

ifdef BUILD_LINUX_LE_arm
TARGET 	        = $(VERSION_NAME).elf
TARGET_FINAL    = $(VERSION_NAME)
TARGET_PLATFORM = LINUX
BUILD_LINUX     = 1
BUILD_SDL       = 1
BUILD_GFX       = 1
BUILD_PTHREAD   = 1
BUILD_SDL_IO    = 1
BUILD_VORBIS    = 1
BUILD_WEBM      = 1
BUILDING        = 1
INCLUDES        = $(SDKPATH)/usr/include \
                  $(SDKPATH)/usr/include/SDL2
OBJTYPE         = elf
LIBRARIES       = $(SDKPATH)/usr/lib
# Workaround for GCC 9
CFLAGS          += -Wno-error=format-overflow -Wno-error=implicit-function-declaration -Wno-error=unused-variable -Wno-error=unused-label -Wno-error=stringop-overflow
endif

ifdef BUILD_DARWIN
TARGET          = $(VERSION_NAME).elf
TARGET_FINAL    = $(VERSION_NAME)
TARGET_PLATFORM = DARWIN
BUILD_SDL       = 1
BUILD_GFX       = 1
BUILD_LINUX     = 1
BUILD_PTHREAD   = 1
BUILD_SDL_IO    = 1
BUILD_OPENGL    = 1
BUILD_LOADGL    = 1
BUILD_VORBIS    = 1
BUILDING        = 1
YASM            = yasm
CC              = $(PREFIX)gcc
OBJTYPE         = macho
INCLUDES        = $(DWNDEV)/include \
                  $(DWNDEV)/include/SDL \
                  $(SDKPATH)/usr/include/malloc
LIBRARIES       = $(DWNDEV)/lib
ifeq ($(findstring darwin10, $(PREFIX)), darwin10)
ARCHFLAGS       = -arch i386 -arch x86_64
else
BUILD_MMX       = 1
TARGET_ARCH     = x86
ARCHFLAGS       = -arch i386
endif
ifeq ($(BUILD_DARWIN), 0)
BUILD_DEBUG     = 1
endif
endif

ifdef BUILD_WII
TARGET 	        = $(VERSION_NAME).elf
TARGET_MAP      = $(TARGET).map
TARGET_FINAL    = boot.dol
TARGET_PLATFORM = WII
BUILD_TREMOR    = 1
BUILD_WEBM      = 1
BUILD_ELM       = 1
BUILDING        = 1
INCLUDES        = $(DEVKITPRO)/portlibs/ppc/include \
                  $(DEVKITPRO)/libogc/include
LIBRARIES       = $(DEVKITPRO)/portlibs/ppc/lib \
                  $(DEVKITPRO)/libogc/lib/wii
ifeq ($(BUILD_WII), 0)
BUILD_DEBUG     = 1
endif
endif

STRIP           = cp $(TARGET) $(TARGET_FINAL)
ifndef BUILD_DEBUG
ifndef NO_STRIP
ifdef BUILD_WIN
STRIP 	        = $(WINDEV)/$(PREFIX)strip$(EXTENSION) $(TARGET) -o $(TARGET_FINAL)
endif
ifdef BUILD_LINUX
STRIP 	        = $(LNXDEV)/$(PREFIX)strip $(TARGET) -o $(TARGET_FINAL)
endif
ifdef BUILD_DARWIN
STRIP           = $(PREFIX)strip $(TARGET) -o $(TARGET_FINAL)
endif
ifdef BUILD_WII
ELF2DOL         = elf2dol $< $@
endif
endif
endif


#----------------------------------------------------------------------------------------------------
# Directories
#----------------------------------------------------------------------------------------------------

ifdef BUILD_WII
INCS           += wii
endif

ifdef BUILD_SDL
INCS           += sdl
endif

ifdef BUILD_LINUX
INCS           += 'libpng-config --prefix'/include/libpng
endif


INCS 	       += .                                                                                 \
                  source                                                                            \
                  source/adpcmlib                                                                   \
                  source/gamelib                                                                    \
                  source/preprocessorlib                                                            \
                  source/ramlib                                                                     \
                  source/randlib                                                                    \
                  source/scriptlib                                                                  \
                  source/openborscript                                                              \
                  source/pnglib                                                                     \


ifdef BUILD_GFX
INCS 	       += source/gfxlib
endif

ifdef BUILD_WEBM
INCS 	       += source/webmlib
endif

INCS += $(INCLUDES)

#----------------------------------------------------------------------------------------------------
# Objects
#----------------------------------------------------------------------------------------------------

ADPCM 	        = source/adpcmlib/adpcm.o

ifdef BUILD_GFX
GFX 	        = source/gfxlib/bilinear.o                                                          \
                  source/gfxlib/dotmatrix.o                                                         \
                  source/gfxlib/gfx.o                                                               \
                  source/gfxlib/scale2x.o                                                           \
                  source/gfxlib/scanline.o                                                          \
                  source/gfxlib/simple2x.o                                                          \
                  source/gfxlib/tv2x.o
endif

ifdef BUILD_MMX
GFX 	       += source/gfxlib/bilinearmmx.o
endif

GAME	        = source/gamelib/draw.o                                                             \
                  source/gamelib/draw16.o                                                           \
                  source/gamelib/draw32.o                                                           \
                  source/gamelib/font.o                                                             \
                  source/gamelib/translation.o                                                             \
                  source/gamelib/anigif.o                                                           \
                  source/gamelib/bitmap.o 	                                                        \
                  source/gamelib/screen.o                                                           \
                  source/gamelib/screen16.o                                                         \
                  source/gamelib/screen32.o                                                         \
                  source/gamelib/loadimg.o                                                          \
                  source/gamelib/palette.o                                                          \
                  source/gamelib/packfile.o                                                         \
                  source/gamelib/filecache.o                                                        \
                  source/gamelib/pixelformat.o                                                      \
                  source/gamelib/soundmix.o                                                         \
                  source/gamelib/spriteq.o                                                          \
                  source/gamelib/sprite.o                                                          \
                  source/gamelib/spritex8p16.o                                                      \
                  source/gamelib/spritex8p32.o                                                      \
                  source/gamelib/models.o                                                           \
                  source/gamelib/transform.o
SCRIPT          = source/scriptlib/StackedSymbolTable.o                                             \
                  source/scriptlib/ScriptVariant.o                                                  \
                  source/scriptlib/SymbolTable.o                                                    \
                  source/scriptlib/Instruction.o                                                    \
                  source/scriptlib/Interpreter.o                                                    \
                  source/scriptlib/ImportCache.o                                                    \
                  source/scriptlib/ParserSet.o                                                      \
                  source/scriptlib/Parser.o                                                         \
                  source/scriptlib/Lexer.o                                                          \
                  source/scriptlib/Stack.o                                                          \
                  source/scriptlib/List.o                                                           \
                  source/preprocessorlib/pp_lexer.o                                                 \
                  source/preprocessorlib/pp_parser.o                                                \
                  source/preprocessorlib/pp_expr.o
SCRIPT_BUILTINS = source/openborscript/commands.o                                                   \
				  source/openborscript/animation.o                                                  \
                  source/openborscript/audio.o                                           			\
				  source/openborscript/axis.o                                           			\
				  source/openborscript/binding.o                                           			\
				  source/openborscript/drawmethod.o                                           		\
                  source/openborscript/collision_attack.o                                           \
                  source/openborscript/common_property.o                                            \
                  source/openborscript/collision_body.o                                             \
				  source/openborscript/collision_entity.o                                           \
                  source/openborscript/colorset.o                                                   \
                  source/openborscript/constants.o                                                  \
                  source/openborscript/entity.o                                                   	\
                  source/openborscript/faction.o                                                   	\
                  source/openborscript/flash.o                                             	        \
                  source/openborscript/global_config.o                                             	\
                  source/openborscript/icon.o                                                       \
                  source/openborscript/index.o                                                      \
                  source/openborscript/layer.o                                                      \
                  source/openborscript/level.o                                                      \
                  source/openborscript/math.o                                                       \
                  source/openborscript/model.o                                                      \
                  source/openborscript/spawn_hud.o                                                  \
                  source/openborscript/status_dial.o                                               \
				  source/openborscript/recursive_damage.o											\
                  source/openborscript/string.o
RAM             = source/ramlib/ram.o
RAND	        = source/randlib/rand32.o
PNG             = source/pnglib/pngdec.o                                                            \
                  source/pnglib/savepng.o
SOURCE	        = source/stringptr.o                                                                \
				  source/utils.o                                                                    \
                  source/stristr.o																	\
                  source/omath.o



ifdef BUILD_PSP
GAME_CONSOLE    = psp/control/control.o                                                             \
                  psp/dvemgr/dvemgr.o                                                               \
                  psp/kernel/kernel.o                                                               \
                  psp/graphics.o                                                                    \
                  psp/audiodrv.o                                                                    \
                  psp/sblaster.o                                                                    \
                  psp/control.o                                                                     \
                  psp/vertex.o                                                                      \
                  psp/timer.o                                                                       \
                  psp/video.o                                                                       \
                  psp/image.o                                                                       \
                  psp/menu.o                                                                        \
                  psp/pspport.o
endif


ifdef BUILD_WII
GAME_CONSOLE    = wii/control.o                                                                     \
                  wii/sblaster.o                                                                    \
                  wii/timer.o                                                                       \
                  wii/video.o                                                                       \
                  wii/threads.o                                                                     \
                  wii/menu.o                                                                        \
                  wii/wiiport.o
endif

ifdef BUILD_VITA
GAME_CONSOLE    = vita/control.o                                                                     \
                  vita/sblaster.o                                                                    \
                  vita/timer.o                                                                       \
                  vita/video.o                                                                       \
                  vita/vitaport.o                                                                    \
                  vita/menu.o
endif


ifdef BUILD_SDL
GAME	       += source/gamelib/filters.o
endif



ifdef BUILD_SDL_IO
GAME_CONSOLE   += sdl/joysticks.o                                                                   \
                  sdl/control.o                                                                     \
                  sdl/sblaster.o                                                                    \
                  sdl/timer.o                                                                       \
                  sdl/sdlport.o                                                                     \
                  sdl/video.o                                                                       \
                  sdl/videocommon.o                                                                 \
				  sdl/threads.o                                                                     \
                  sdl/menu.o
endif


ifdef BUILD_OPENGL
GAME_CONSOLE   += sdl/opengl.o
endif


ifdef BUILD_LOADGL
GAME_CONSOLE   += sdl/loadgl.o
endif


ifdef BUILD_WEBM
WEBM 	       += source/webmlib/vidplay.o                                                          \
                  source/webmlib/vorbis.o                                                           \
				  source/webmlib/samplecvt.o                                                        \
                  source/webmlib/yuv.o                                                              \
				  source/webmlib/halloc/halloc.o                                                    \
				  source/webmlib/nestegg/nestegg.o
endif


MAIN            = openbor.o					                                                \
                  openborscript.o

OBJS            = $(MAIN)                                                                           \
                  $(GAME_CONSOLE)                                                                   \
                  $(ADPCM)                                                                          \
                  $(GFX)                                                                            \
                  $(GAME)                                                                           \
                  $(SOURCE)                                                                         \
                  $(SCRIPT)                                                                         \
                  $(SCRIPT_BUILTINS)                                                                \
                  $(RAM)                                                                            \
                  $(RAND)                                                                           \
                  $(PNG)                                                                            \
                  $(WEBM)


#----------------------------------------------------------------------------------------------------
# Compiler Flags
#----------------------------------------------------------------------------------------------------

CFLAGS 	       += $(addprefix -I", $(addsuffix ", $(INCS))) $(ARCHFLAGS) -D$(TARGET_PLATFORM)
CFLAGS 	       += -g -Wall -Werror -Wno-format-truncation -Wno-stringop-truncation -fsigned-char -std=gnu99


ifndef BUILD_DARWIN
ifdef BUILD_LINUX
CFLAGS         += -Wno-unused-result `pkg-config sdl2 --cflags`
endif
endif

ifndef BUILD_DEBUG
CFLAGS 	       += -O2
CFLAGS 	       += -fno-ident -freorder-blocks
ifndef BUILD_AMD64
CFLAGS         += -fomit-frame-pointer
endif
else
CFLAGS 	       += -DDEBUG -O0
ifdef NO_RAM_DEBUGGER
CFLAGS         += -DNO_RAM_DEBUGGER
endif
endif


ifdef BUILD_PSP
CFLAGS         += -G0
endif


ifdef BUILD_SDL
CFLAGS 	       += -DSDL=1
endif


ifdef BUILD_DARWIN
CFLAGS 	       += -DLINUX -headerpad_max_install_names -isysroot $(SDKPATH)
endif


ifdef BUILD_PANDORA
CFLAGS         += -DLINUX
endif


ifdef BUILD_WII
CFLAGS 	       += -DCACHE_BACKGROUNDS -DREVERSE_COLOR -D__ppc__ $(MACHDEP) -Wl,-Map,$(TARGET_MAP),-wrap,wiiuse_register
# This allows to compile with DevkitPPC r29 and above.
CFLAGS 	       += -U__INT32_TYPE__ -U __UINT32_TYPE__ -D__INT32_TYPE__=int
endif


ifdef BUILD_VITA
CFLAGS 	       += -DCACHE_BACKGROUNDS -Wl,-q
endif


ifdef BUILD_MMX
CFLAGS 	       += -DMMX
endif


ifdef BUILD_VORBIS
CFLAGS         += -DOV_EXCLUDE_STATIC_CALLBACKS
endif


ifdef BUILD_TREMOR
CFLAGS         += -DTREMOR
endif


ifdef BUILD_OPENGL
CFLAGS         += -DOPENGL
endif


ifdef BUILD_LOADGL
CFLAGS         += -DLOADGL
endif


ifdef BUILD_GLES
CFLAGS         += -DGLES
endif


ifdef BUILD_VERBOSE
CFLAGS         += -DVERBOSE
endif


ifdef BUILD_WEBM
CFLAGS 	       += -DWEBM
endif


CXXFLAGS        = $(CFLAGS) -fno-exceptions -fno-rtti
ASFLAGS         = $(CFLAGS)

#----------------------------------------------------------------------------------------------------
# Library
#----------------------------------------------------------------------------------------------------

LIBS            = $(addprefix -L", $(addsuffix ", $(LIBRARIES)))


ifdef BUILD_PSP
LIBS 	       += -lpspgu -lpspaudio -lpsppower -lpsprtc
endif


ifdef BUILD_DARWIN
LIBS           += -Wl,-syslibroot,$(SDKPATH) \
                  -framework Cocoa \
                  -framework OpenGL \
                  -framework Carbon \
                  -framework AudioUnit \
                  -framework IOKit \
                  -lSDLmain
endif


ifdef BUILD_SDL
ifeq ($(findstring DGE, $(SDKPATH)), DGE)
LIBS           += -lSDL -lts
else
COMMA		=,
RPATH_LIBS	= $(addprefix -Wl$(COMMA)-rpath$(COMMA),"$(LIBRARIES)")
LIBS	       += $(RPATH_LIBS) -lSDL2

ifdef CROSSCOMPILE_LINUX_WIN
LIBS	       += -lsetupapi
endif

endif
endif


ifdef BUILD_WIN
LIBS           += -lpsapi -lopengl32 -lwinmm -lole32 -loleaut32 -luuid -limm32 -lversion -mwindows
endif


ifdef BUILD_PTHREAD
LIBS           += -lpthread
endif


ifdef BUILD_WII
LIBS           += -lwupc -lwiiuse -lbte -lfat -lasnd -logc
endif


ifdef BUILD_VITA
LIBS           += -lvita2d -lSceKernel_stub -lSceDisplay_stub -lSceGxm_stub \
	              -lSceSysmodule_stub -lSceCtrl_stub -lScePgf_stub \
	              -lSceCommonDialog_stub -lSceAudio_stub -lfreetype -ljpeg
endif


ifdef BUILD_STATIC
LIBS           += -static
endif


ifdef BUILD_VORBIS
LIBS           += -lvorbisfile -lvorbis -logg
endif


ifdef BUILD_TREMOR
LIBS           += -lvorbisidec -logg
endif


ifdef BUILD_WEBM
LIBS           += -lvpx
ifdef CROSSCOMPILE_LINUX_WIN
LIBS	       += -lpthread
endif
endif


LIBS           += -lpng -lz -lm

#----------------------------------------------------------------------------------------------------
# Rules to manage Files and Libraries for PSP
#----------------------------------------------------------------------------------------------------

ifdef BUILD_PSP
%.o : %.c
	@echo Compiling $(TARGET_PLATFORM) Port: $<...
	@$(CC) $(CFLAGS) -c $< -o $@
%.o : %.S
	@echo Compiling $(TARGET_PLATFORM) Port: $<...
	@$(CC) $(CFLAGS) -c $< -o $@
INCDIR          = $(INCS)
PSP_EBOOT_TITLE = $(VERSION_NAME) $(VERSION)
PSP_EBOOT_ICON 	= resources/OpenBOR_Icon_144x80.png
PSP_EBOOT_PIC1	= resources/OpenBOR_Logo_480x272.png
PSP_FW_VERSION  = 371
PSP_LARGE_MEMORY= 1
BUILD_PRX       = 1
include psp/build.mak
endif



#----------------------------------------------------------------------------------------------------
# Rules to manage Files and Libraries for SDL
#----------------------------------------------------------------------------------------------------

ifdef BUILD_SDL
all : $(TARGET) $(TARGET_FINAL)
	@echo
%.o : %.asm
	@echo Compiling $(TARGET_PLATFORM) Port: $<...
	@$(YASM) -D $(TARGET_PLATFORM) -f $(OBJTYPE) -m $(TARGET_ARCH) -o $@ $<
%.o : %.c
	@echo Compiling $(TARGET_PLATFORM) Port: $<...
	@$(CC) $(CFLAGS) -c $< -o $@
$(TARGET) : $(OBJS) $(RES)
	@echo
	@echo Linking $(TARGET_PLATFORM) Port: $(TARGET)...
	@$(CC) $(CFLAGS) -o $(TARGET) $(OBJS) $(TARGET_RESOURCE) $(LIBS)
$(TARGET_FINAL) : $(TARGET)
	@echo Stripping $(TARGET_PLATFORM) Port: $(TARGET_FINAL)...
	@$(STRIP)
	@echo
	@echo Completed $(TARGET_PLATFORM) Port!
	@echo $(TARGET_FINAL) is now ready!
endif

#----------------------------------------------------------------------------------------------------
# Rules to manage Files and Libraries for Wii
#----------------------------------------------------------------------------------------------------

ifdef BUILD_WII
SOURCES = $(INCS)
include $(DEVKITPPC)/wii_rules
all : $(TARGET) $(TARGET_FINAL)
	@echo
%.o : %.c
	@echo Compiling $(TARGET_PLATFORM) Port: $<...
	@$(CC) $(CFLAGS) -c $< -o $@
$(TARGET) : $(OBJS) $(RES)
	@echo
	@echo Linking $(TARGET_PLATFORM) Port: $(TARGET)...
	@$(CC) $(CFLAGS) -o $(TARGET) $(OBJS) $(TARGET_RESOURCE) $(LIBS)
$(TARGET_FINAL) : $(TARGET)
	@echo Stripping $(TARGET_PLATFORM) Port: $(TARGET_FINAL)...
	@$(ELF2DOL)
	@echo
	@echo Completed $(TARGET_PLATFORM) Port!
	@echo $(TARGET_FINAL) is now ready!
endif

#----------------------------------------------------------------------------------------------------
# Rules to manage Files and Libraries for Vita
#----------------------------------------------------------------------------------------------------

ifdef BUILD_VITA
all : OpenBOR.vpk
	@echo
%.o : %.c
	@echo Compiling $(TARGET_PLATFORM) Port: $<...
	@$(CC) $(CFLAGS) -c $< -o $@
OpenBOR.elf : $(OBJS) $(RES)
	@echo
	@echo Linking $(TARGET_PLATFORM) Port: $(TARGET)...
	@$(CC) $(CFLAGS) -o $(TARGET) $(OBJS) $(TARGET_RESOURCE) $(LIBS)
%.velf: %.elf
	vita-elf-create $< $@
eboot.bin: $(VERSION_NAME).velf
	vita-make-fself -s $< $@
OpenBOR.vpk: eboot.bin
	@echo Stripping $(TARGET_PLATFORM) Port: $(TARGET_FINAL)...
	vita-mksfoex -s TITLE_ID=$(TITLE_ID) "OpenBOR" param.sfo
	vita-pack-vpk -s param.sfo -b eboot.bin --add resources/vita_icon.png=sce_sys/icon0.png $@
	@echo
	@echo Completed $(TARGET_PLATFORM) Port!
	@echo $(TARGET_FINAL) is now ready!
endif


#----------------------------------------------------------------------------------------------------
# Rules to CleanUp Files for All Platforms
#----------------------------------------------------------------------------------------------------

ifndef BUILDING
all:
	@echo
	@echo Build A TARGET_PLATFORM:
	@echo
	@echo make BUILD_PSP=1
	@echo make BUILD_PS2=1
	@echo make BUILD_WII=1
	@echo make BUILD_WIN=1
	@echo make BUILD_PANDORA=1
	@echo make BUILD_LINUX=1
	@echo
	@echo Cleanup Intermediate Files:
	@echo
	@echo make clean
	@echo
	@echo Remove All Files:
	@echo
	@echo make clean-all
	@echo
endif


ifndef BUILD_PSP
clean-all: clean-releases clean

clean-releases:
	@if [ -d ./releases ]; then rm -rf ./releases; fi

clean:
	@echo
	@echo "Removing All $(TARGET_PLATFORM) Files..."
	@rm -f $(TARGET) $(TARGET_FINAL) $(TARGET_MAP) PARAM.SFO param.sfo eboot.bin OpenBOR.velf linkmap $(OBJS)
	@echo Done!
	@echo
endif

version:
	@echo "-------------------------------------------------------"
	@echo "OpenBOR $(VERSION) - https://www.ChronoCrash.com"
	@echo
	@echo "All rights reserved."
	@echo "See LICENSE and README within OpenBOR root for details."
	@echo
	@echo "Copyright (c) OpenBOR Team"
	@echo "-------------------------------------------------------"


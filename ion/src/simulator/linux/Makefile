# The following lines allow us to use our own SDL_config.h

# First, make sure an error is raised if we ever use the standard SDL_config.h
SFLAGS += -DUSING_GENERATED_CONFIG_H
# Then use our very own include dir if either SDL.h or SDL_config.h are included
# Note that this one will appear before ion/src/simulator/external/sdl/include
# which is added later on in the SDL Makefile
SFLAGS += -Iion/src/simulator/linux/include

ion_src += $(addprefix ion/src/simulator/linux/, \
  images.cpp \
  language.cpp \
  assets.s \
)

ion_src += $(addprefix ion/src/simulator/shared/, \
  dummy/callback.cpp \
  dummy/haptics_enabled.cpp \
  collect_registers_x86_64.s \
  collect_registers.cpp \
  haptics.cpp \
)

ifeq ($(EPSILON_TELEMETRY),1)
ion_src += ion/src/simulator/shared/dummy/telemetry_init.cpp
ion_src += ion/src/shared/telemetry_console.cpp
endif

LDFLAGS += -ljpeg

$(eval $(call rule_for, \
  INCBIN, \
  ion/src/simulator/linux/assets.s ion/src/simulator/linux/images.h, \
  $(ion_simulator_assets_paths), \
  $$(PYTHON) ion/src/simulator/linux/incbin.py $(ion_simulator_assets) -o $$@, \
  global \
))

$(call object_for,ion/src/simulator/linux/images.cpp): $(BUILD_DIR)/ion/src/simulator/linux/images.h

# The header is refered to as <ion/src/simulator/linux/images.h> so make sure it's findable this way
SFLAGS += -I$(BUILD_DIR)

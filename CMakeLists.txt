cmake_minimum_required(VERSION 3.15)

project(DJApp VERSION 1.0.0)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Add executable
add_executable(DJApp
    Source/Main.cpp
    
    # Model files
    Source/Model/Track.cpp
    Source/Model/PlaylistManager.cpp
    Source/Model/AudioEngine.cpp
    
    # Controller files
    Source/Controller/DJController.cpp
    
    # Component files
    Source/Components/ModernButton.cpp
    Source/Components/ModernSlider.cpp
    Source/Components/WaveformView.cpp
    Source/Components/VUMeter.cpp
    Source/Components/JogWheel.cpp
    
    # View files
    Source/View/MainView.cpp
    Source/View/DeckView.cpp
    Source/View/MixerView.cpp
    Source/View/PlaylistView.cpp
)

# Include directories
target_include_directories(DJApp PRIVATE
    Source
    Source/Model
    Source/Controller
    Source/Components
    Source/View
    Source/Utils
)

# Try to find JUCE installation
if(APPLE)
    # Common JUCE installation paths on macOS
    set(JUCE_PATHS
        "/usr/local/include/JUCE"
        "/opt/homebrew/include/JUCE"
        "~/JUCE/modules"
        "/Applications/JUCE/modules"
    )
    
    foreach(JUCE_PATH ${JUCE_PATHS})
        if(EXISTS ${JUCE_PATH})
            target_include_directories(DJApp PRIVATE ${JUCE_PATH})
            break()
        endif()
    endforeach()
    
    # Link macOS frameworks
    target_link_libraries(DJApp PRIVATE
        "-framework Cocoa"
        "-framework CoreAudio"
        "-framework CoreMIDI"
        "-framework AudioUnit"
        "-framework AudioToolbox"
        "-framework Accelerate"
        "-framework QuartzCore"
        "-framework OpenGL"
    )
endif()

# Compiler flags
target_compile_definitions(DJApp PRIVATE
    JUCE_WEB_BROWSER=0
    JUCE_USE_CURL=0
    JUCE_APPLICATION_NAME_STRING="\"DJApp\""
    JUCE_APPLICATION_VERSION_STRING="\"1.0.0\""
)

if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    target_compile_definitions(DJApp PRIVATE DEBUG=1 _DEBUG=1)
else()
    target_compile_definitions(DJApp PRIVATE NDEBUG=1)
endif()
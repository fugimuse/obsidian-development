# Video Podcast Hardware Setup Guide

## Overview
This guide covers the hardware setup and configuration for professional video podcast production, extracted from the comprehensive development machine setup procedure.

---

## Hardware Requirements

### Audio Equipment

**Primary Microphone Setup:**
- **Recommended**: Audio-Technica AT2020USB+ or Shure SM7B with audio interface
- **Budget option**: Blue Yeti or Audio-Technica ATR2100x-USB
- **Windscreen/Pop filter**: Essential for professional audio quality
- **Boom arm**: Reduces handling noise and improves positioning

**Audio Interface (if using XLR mics):**
- **Professional**: Focusrite Scarlett 2i2 (3rd Gen) or PreSonus AudioBox USB 96
- **Multi-person setup**: Zoom PodTrak P4 or Rode RodeCaster Pro

**Monitoring:**
- **Headphones**: Sony MDR-7506 or Audio-Technica ATH-M50x
- **Studio monitors** (optional): KRK Rokit 5 G4 or Yamaha HS5

### Video Equipment

**Camera Setup:**
- **Primary camera**: Sony A6400, Canon EOS M50 Mark II, or Logitech C920 Pro (budget)
- **Secondary camera** (optional): For multi-angle shots
- **Tripods**: Sturdy tripods for each camera position
- **Camera mounting**: HDMI capture cards for DSLR/mirrorless cameras

**Capture Hardware:**
- **HDMI capture card**: Elgato Cam Link 4K or BlackMagic Design ATEM Mini
- **USB hubs**: Powered USB 3.0 hubs for multiple devices
- **Storage**: Fast external SSD for recording (Samsung T7 or SanDisk Extreme Pro)

### Lighting Equipment

**Basic Three-Point Lighting:**
- **Key light**: Softbox with LED panel (Neewer 660 LED or Godox SL-60W)
- **Fill light**: Smaller LED panel or reflector
- **Background light**: LED strip or small spotlight for separation

**Modifiers:**
- **Softboxes**: For diffused, flattering light
- **Reflectors**: 5-in-1 reflector kit for fill and bounce
- **Light stands**: Adjustable stands for proper positioning

### Streaming/Recording Hardware

**Computer Specifications:**
- **Minimum**: Intel i5-8400 / AMD Ryzen 5 3600, 16GB RAM, dedicated GPU
- **Recommended**: Intel i7-10700K / AMD Ryzen 7 3700X, 32GB RAM, RTX 3060 or better
- **Storage**: SSD for OS and recording, separate drive for long-term storage

**Network:**
- **Ethernet connection**: Wired internet for stable streaming
- **Minimum bandwidth**: 10 Mbps upload for 1080p streaming
- **Backup internet**: Mobile hotspot or secondary connection

---

## Software Configuration

### Recording Software

**OBS Studio Setup:**
```bash
# Install OBS Studio
sudo pacman -S obs-studio

# Essential OBS plugins
yay -S obs-studio-plugin-browser
yay -S obs-studio-plugin-input-overlay
```

**OBS Configuration:**
- **Video settings**: 1920x1080, 60fps (or 30fps for streaming)
- **Audio settings**: 48kHz sample rate, 320kbps bitrate
- **Recording format**: MP4 with H.264 encoding
- **Streaming settings**: CBR bitrate based on upload speed

**Alternative Software:**
- **Restream Studio**: Browser-based streaming
- **Riverside.fm**: High-quality remote recording
- **SquadCast**: Remote podcast recording

### Audio Processing

**Real-time Audio Processing:**
```bash
# Install audio tools
sudo pacman -S pulseaudio-jack jack2 qjackctl
yay -S carla

# Noise reduction plugins
yay -S noise-repellent-lv2
```

**Post-production Software:**
```bash
# Install Audacity for audio editing
sudo pacman -S audacity

# Professional option: Ardour
sudo pacman -S ardour
```

### Video Editing

**DaVinci Resolve Setup:**
```bash
# Download from BlackMagic Design website
# Install dependencies
sudo pacman -S opencl-mesa lib32-opencl-mesa

# Alternative: Kdenlive
sudo pacman -S kdenlive
```

---

## Physical Setup

### Studio Layout

**Camera Positioning:**
- **Primary camera**: Eye level, 3-6 feet from subject
- **Secondary camera**: 45-90 degree angle from primary
- **Wide shot**: Overview of entire setup (optional)

**Audio Positioning:**
- **Microphone**: 6-12 inches from mouth, slightly off-axis
- **Acoustic treatment**: Foam panels or moving blankets to reduce echo
- **Quiet space**: Minimize background noise and room reflections

### Cable Management

**Power Management:**
- **UPS/Power strips**: Protect equipment from power issues
- **Cable routing**: Keep power and audio cables separated
- **Labeling**: Label all cables for easy troubleshooting

**Signal Path:**
- **Audio**: Mic → Interface → Computer → Monitoring
- **Video**: Camera → Capture card → Computer → Display/Stream
- **Backup recording**: Always record locally while streaming

---

## Workflow Setup

### Pre-recording Checklist

**Technical Setup:**
- [ ] All equipment powered on and connected
- [ ] Audio levels checked and recording test completed
- [ ] Video framing and focus confirmed
- [ ] Lighting levels adjusted
- [ ] Background and set dressed
- [ ] Recording software configured and tested
- [ ] Backup recording started

**Content Preparation:**
- [ ] Talking points or script prepared
- [ ] Guest connection tested (if applicable)
- [ ] Screen sharing or presentation materials ready
- [ ] Water and comfort items available

### Recording Workflow

**Directory Structure:**
```bash
mkdir -p ~/Podcasts/{Raw,Edited,Published,Assets}
mkdir -p ~/Podcasts/Raw/{Audio,Video,Backups}
mkdir -p ~/Podcasts/Assets/{Graphics,Music,SFX}
```

**File Naming Convention:**
```
YYYY-MM-DD_Episode-XXX_Guest-Name_Version
2024-03-15_Episode-042_John-Smith_Raw-Audio.wav
2024-03-15_Episode-042_John-Smith_Final-Mix.mp3
```

### Post-production Pipeline

**Audio Processing:**
1. **Sync audio** from interface with camera audio
2. **Noise reduction** and EQ adjustments
3. **Level balancing** between speakers
4. **Add intro/outro** music and branding
5. **Export final audio** in multiple formats

**Video Processing:**
1. **Sync multiple camera angles** with audio
2. **Color correction** and exposure matching
3. **Cut editing** for pacing and content
4. **Add graphics, titles, and lower thirds**
5. **Export for different platforms** (YouTube, podcast, clips)

---

## Optimization and Maintenance

### System Optimization

**Linux Audio Optimization:**
```bash
# Set real-time permissions
sudo gpasswd -a $USER audio
sudo gpasswd -a $USER realtime

# Optimize system for low-latency audio
echo '@audio - rtprio 95' | sudo tee -a /etc/security/limits.conf
echo '@audio - memlock unlimited' | sudo tee -a /etc/security/limits.conf
```

**GPU Acceleration:**
```bash
# Enable hardware encoding for faster exports
# AMD
sudo pacman -S mesa-vdpau libva-mesa-driver

# NVIDIA
sudo pacman -S nvidia-utils lib32-nvidia-utils
```

### Regular Maintenance

**Equipment Care:**
- **Clean camera lenses** and filters regularly
- **Check cable connections** and replace worn cables
- **Update firmware** on cameras and audio interfaces
- **Calibrate monitors** for accurate color reproduction

**Software Updates:**
- **Keep OBS updated** for latest features and bug fixes
- **Update video drivers** for optimal performance
- **Backup configurations** before major updates
- **Test setup** after any system changes

---

## Troubleshooting

### Common Audio Issues

**No audio input:**
```bash
# Check audio devices
arecord -l
pacmd list-sources

# Reset PulseAudio
pulseaudio -k
pulseaudio --start
```

**Audio sync issues:**
- **Use clapper board** or hand clap for sync reference
- **Check sample rates** match across all devices
- **Monitor latency** in OBS audio advanced properties

### Common Video Issues

**Camera not detected:**
```bash
# Check USB devices
lsusb

# Check video devices
v4l2-ctl --list-devices

# Reset USB device
sudo modprobe -r uvcvideo
sudo modprobe uvcvideo
```

**Performance issues:**
- **Lower recording resolution** temporarily
- **Close unnecessary applications**
- **Check CPU and GPU usage** during recording
- **Ensure adequate storage space** for recordings

---

## Platform-Specific Settings

### YouTube Optimization

**Upload Settings:**
- **Resolution**: 1920x1080 or 3840x2160 (4K)
- **Frame rate**: 24, 30, or 60fps consistently
- **Bitrate**: 8-12 Mbps for 1080p, 35-45 Mbps for 4K
- **Audio**: 320kbps AAC, stereo or mono depending on content

### Podcast Platform Settings

**Audio Specifications:**
- **Format**: MP3 at 128-320kbps or WAV for highest quality
- **Sample rate**: 44.1kHz or 48kHz
- **Mono vs Stereo**: Mono for single speaker, stereo for multiple
- **Loudness**: -16 LUFS for podcasts, -14 LUFS for music content

---

## Budget Considerations

### Starter Setup ($500-800)
- **Microphone**: Blue Yeti or Audio-Technica ATR2100x-USB
- **Camera**: Logitech C920 Pro or C922
- **Lighting**: Basic LED panel kit
- **Software**: OBS Studio (free)

### Professional Setup ($2000-5000)
- **Microphone**: Shure SM7B with Focusrite Scarlett Solo
- **Camera**: Sony A6400 with Elgato Cam Link 4K
- **Lighting**: Three-point LED lighting kit with softboxes
- **Audio interface**: Zoom PodTrak P4 or RodeCaster Pro

### Enterprise Setup ($5000+)
- **Multiple cameras**: Sony FX3 or Canon C70
- **Professional audio**: RodeCaster Pro II with multiple mics
- **Advanced lighting**: Professional LED panels with DMX control
- **Streaming hardware**: ATEM Mini Extreme or dedicated streaming PC

This hardware setup guide provides everything needed to establish a professional video podcast production environment, from basic setups to broadcast-quality configurations.
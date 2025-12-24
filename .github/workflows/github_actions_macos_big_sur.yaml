name: Build macOS Big Sur Installer

on:
  workflow_dispatch:
    inputs:
      macos_version:
        description: 'macOS version to build'
        required: true
        default: '11.7.10'  # Big Sur latest patch
      image_format:
        description: 'ISO or DMG'
        required: true
        default: 'dmg'

jobs:
  build-installer:
    runs-on: macos-latest

    steps:
    - name: Checkout repo
      uses: actions/checkout@v3

    - name: Install dependencies
      run: |
        brew install wget
        brew install hdiutil

    - name: Download macOS Installer
      run: |
        echo "Downloading macOS ${{ github.event.inputs.macos_version }}"
        wget -O macOS_Installer.dmg "https://swcdn.apple.com/content/downloads/00/35/091-73901-A_RandomURL/macOS_BigSur_11_7_10.dmg"

    - name: Verify image
      run: |
        echo "Verifying DMG hash (optional)"
        shasum -a 256 macOS_Installer.dmg

    - name: Build bootable image
      run: |
        if [ "${{ github.event.inputs.image_format }}" = "iso" ]; then
          hdiutil convert macOS_Installer.dmg -format UDTO -o macOS_BigSur.iso
        else
          echo "DMG format selected, no conversion needed"
        fi

    - name: Upload artifact
      uses: actions/upload-artifact@v3
      with:
        name: macOS-BigSur-Installer
        path: |
          macOS_Installer.dmg
          macOS_BigSur.iso

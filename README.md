# ⚠️ THIS DOESN'T WORK, USE THIS INSTEAD 👇
👉 [Win Download](https://download.drmare.com/MusicConverter.exe) 
👉 [Mac Download](https://download.drmare.com/MusicConverter.dmg)

**Use Zotify at your own risk. This fork is for educational and research purposes only.**

---

## ⚠️ Important Notice (2024-2025 Update)

**Spotify strengthened its security measures in mid-2024, making Zotify usage more complex.**

### 🔴 Core Requirements

- 💎 **Spotify Premium Subscription Required** ← Most Important!
  - ❌ Free accounts will encounter `Audio key error, code: 1`
  - ✅ Only Premium accounts can download successfully
  - 💡 You can subscribe for 1 month, download, then cancel

- ❌ **No longer supports** direct username/password login
- ✅ **Must use** `credentials.json` file for authentication
- ⚙️ **Requires additional tools**: librespot-auth to generate credentials file
- 🔧 **Complex configuration**: Requires Rust compiler and multiple steps

### 🎯 If You Don't Have Premium

- 🔀 **Free account fork**: https://github.com/bgeorgakas/zotify (160kbps, requires 30s delay)
- 🌟 **Recommended alternative**: [SpotDL](https://github.com/spotDL/spotify-downloader) - No Spotify account needed (see end of document)
- 💼 **Commercial solution**: [DRmare Spotify Music Converter](https://www.drmare.com/spotify-music-converter/) - Safest option

---

## Tool Introduction

Zotify is a highly customizable music and podcast downloader that can download audio content from Spotify.

**Official Repository**: https://github.com/zotify-dev/zotify

![Zotify Logo](https://i.imgur.com/hGXQWSl.png)

## Main Features

- 💎 **Requires Spotify Premium Subscription** (official version)
  - ❌ Free accounts cannot download (will get Audio key error)
  - 🔀 Alternative: Use the [fork version](https://github.com/bgeorgakas/zotify) that supports free accounts

- ✅ Supports up to 320kbps downloads (Premium accounts)
- ✅ Direct downloads from Spotify source (not third-party sources like YouTube or Deezer)
- ✅ Supports downloading podcasts, playlists, liked songs, albums, artists, singles
- ✅ Downloads synced lyrics (.lrc format, uses unsynced lyrics when unavailable)
- ✅ Optional real-time download mode (downloads at normal playback speed, appears more legitimate)
- ✅ Supports multiple audio formats (mp3, ogg, opus, etc.)
- ✅ Direct download via URL or built-in search function
- ✅ Bulk download from text file with URL list

## System Requirements

### Required Dependencies

1. 💎 **Spotify Premium Subscription** ⚠️ **Most Important!**
   - Official Zotify requires Premium
   - Free accounts should use [fork version](https://github.com/bgeorgakas/zotify) or SpotDL

2. **Python**: 3.9 or higher
3. **FFmpeg**: Audio conversion tool
4. **Rust Toolchain** (for compiling librespot-auth) ⚠️ New requirement
5. **Git** (for cloning repositories)

### Installing Dependencies

#### Installing Python
Visit [Python official website](https://www.python.org/) to download and install Python 3.9+

#### Installing FFmpeg

**Windows**:
- Download FFmpeg: https://ffmpeg.org/download.html
- Add FFmpeg to system PATH environment variable

**macOS**:
```bash
brew install ffmpeg
```

**Linux**:
```bash
sudo apt install ffmpeg  # Ubuntu/Debian
sudo yum install ffmpeg  # CentOS/RHEL
```

#### Installing Rust (⚠️ Required)

**Windows**:
- Visit https://www.rust-lang.org/tools/install
- Download and run `rustup-init.exe`
- Select default installation

**macOS/Linux**:
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Verify installation:
```bash
rustc --version
cargo --version
```

## Installing Zotify

### 1. Install Zotify Main Program

```bash
pip install git+https://github.com/zotify-dev/zotify.git
```

### 2. Fix protobuf Version Issue

⚠️ **Must execute**, otherwise errors will occur:

```bash
pip install "protobuf==3.20.1"
```

### 3. Install librespot-auth (Generate Credentials)

⚠️ **Critical step**: Used to generate `credentials.json`

```bash
# Clone repository
git clone https://github.com/dspearson/librespot-auth.git
cd librespot-auth

# Compile (first compilation takes 5-10 minutes)
cargo build --release

# After compilation, executable is located at:
# Windows: .\target\release\librespot-auth.exe
# macOS/Linux: ./target/release/librespot-auth
```

## ⚠️ Critical Step: Generating credentials.json

**This is a required step for using Zotify after 2024!**

### Why Do You Need credentials.json?

Starting from mid-2024:
- ❌ Spotify disabled direct username/password login API
- ✅ Must use OAuth tokens for authentication
- 🔑 librespot-auth obtains valid tokens by simulating a Spotify Connect device

### Generation Steps

#### Step 1: Run librespot-auth

```bash
# Windows
cd librespot-auth
.\target\release\librespot-auth.exe --class computer

# macOS/Linux
cd librespot-auth
./target/release/librespot-auth --class computer
```

**Parameter explanation**:
- `--class computer`: Device type as computer (doesn't require Premium account)
- `--name "device name"`: Custom device name (optional)
- `--path credentials.json`: Specify output file path (optional)

#### Step 2: Authorize from Spotify Client

1. After running librespot-auth, it will display:
   ```
   Open Spotify and select output device: Computer
   ```

2. **On the same network**, open Spotify client (desktop or mobile version)

3. Play any song, select "Computer" or your custom device name from the playback device list

4. After selection, librespot-auth will automatically:
   - ✅ Receive authentication token sent by Spotify
   - ✅ Save to `credentials.json`
   - ✅ Exit automatically

5. Upon success, a `credentials.json` file will be generated in the current directory

**credentials.json format example**:
```json
{
  "username": "your_spotify_user_id",
  "auth_type": 1,
  "auth_data": "Base64_encoded_authentication_token..."
}
```

#### Common Issue: Device Not Found

If Spotify client cannot find the device:

1. **Check network**: Ensure the device running librespot-auth and Spotify client are on the same network
2. **Disable VPN/Proxy**: These may block zeroconf discovery
3. **Disable firewall**: Temporarily disable or add allow rules
4. **Try different device types**: Use `--class speaker` or `--class tablet`
5. **Check Spotify version**: Ensure using the latest client

#### Step 3: Configure Zotify to Use Credentials

**Windows**:
```powershell
# Create config directory
mkdir "$env:APPDATA\Zotify" -Force

# Copy credentials file
copy credentials.json "$env:APPDATA\Zotify\"
```

**macOS**:
```bash
mkdir -p ~/Library/ApplicationSupport/Zotify/
cp credentials.json ~/Library/ApplicationSupport/Zotify/
```

**Linux**:
```bash
mkdir -p ~/.config/zotify/
cp credentials.json ~/.config/zotify/
```

## Configuration File Settings

### Configuration File Location

| OS | Configuration File Path |
|---------|-------------|
| Windows | `C:\Users\<USERNAME>\AppData\Roaming\Zotify\config.json` |
| macOS   | `/Users/<USERNAME>/Library/ApplicationSupport/Zotify/config.json` |
| Linux   | `/home/<USERNAME>/.config/zotify/config.json` |

### ⚠️ Windows Configuration Pitfall

**Very Important**: When configuring paths on Windows, you must use your **real Windows username**, not your Spotify user ID!

**Wrong Example** ❌:
```json
{
  "CREDENTIALS_LOCATION": "C:\\Users\\31fbmrg6zrvkcstxvguhhsqggiky\\AppData\\..."
}
```
(`31fbmrg6zrvkcstxvguhhsqggiky` is a Spotify user ID, not a Windows username!)

**Correct Example** ✅:
```json
{
  "CREDENTIALS_LOCATION": "C:\\Users\\YourWindowsUsername\\AppData\\Roaming\\Zotify\\credentials.json"
}
```

**How to get your real Windows username**:
```powershell
# Method 1
echo $env:USERNAME

# Method 2
whoami
```

### Recommended Configuration (Windows)

Create or edit `C:\Users\<your_username>\AppData\Roaming\Zotify\config.json`:

```json
{
  "SAVE_CREDENTIALS": "True",
  "CREDENTIALS_LOCATION": "C:\\Users\\your_username\\AppData\\Roaming\\Zotify\\credentials.json",
  "OUTPUT": "{artist}\\{album}\\{song_name}.{ext}",
  "ROOT_PATH": "",
  "DOWNLOAD_FORMAT": "mp3",
  "DOWNLOAD_QUALITY": "auto",
  "DOWNLOAD_LYRICS": "True",
  "SKIP_EXISTING": "True",
  "PRINT_SPLASH": "False",
  "PRINT_DOWNLOAD_PROGRESS": "True"
}
```

**Note**:
- ✅ Windows paths must use double backslashes `\\`
- ✅ Replace `your_username` with your real Windows username
- ✅ Also use double backslashes in `OUTPUT` paths

### Complete Configuration Options

| Config Key | Command Line Parameter | Default Value | Description |
|-------|-----------|--------|------|
| CREDENTIALS_LOCATION | --credentials-location | - | **⚠️ Required** Full path to credentials.json |
| OUTPUT | --output | - | Output location/format (see below) |
| SONG_ARCHIVE | --song-archive | - | Archive file for skipping downloaded songs |
| ROOT_PATH | --root-path | - | Directory where Zotify saves music |
| ROOT_PODCAST_PATH | --root-podcast-path | - | Directory where Zotify saves podcasts |
| SPLIT_ALBUM_DISCS | --split-album-discs | False | Save each disc in separate folder |
| DOWNLOAD_LYRICS | --download-lyrics | True | Download synced lyrics (.lrc format) |
| MD_ALLGENRES | --md-allgenres | False | Save all related genres in metadata |
| MD_GENREDELIMITER | --md-genredelimiter | , | Delimiter for separating genres in metadata |
| DOWNLOAD_FORMAT | --download-format | ogg | Download audio format (aac, fdk_aac, m4a, mp3, ogg, opus, vorbis) |
| DOWNLOAD_QUALITY | --download-quality | auto | Audio quality (normal, high, very_high*) |
| TRANSCODE_BITRATE | --transcode-bitrate | auto | Override bitrate for ffmpeg encoding |
| SKIP_EXISTING_FILES | --skip-existing | True | Skip songs with same name |
| SKIP_PREVIOUSLY_DOWNLOADED | --skip-previously-downloaded | False | Use song_archive file to skip previously downloaded songs |
| RETRY_ATTEMPTS | --retry-attempts | 1 | Number of times Zotify retries failed requests |
| BULK_WAIT_TIME | --bulk-wait-time | 1 | Wait time between bulk downloads |
| OVERRIDE_AUTO_WAIT | --override-auto-wait | False | Completely disable wait time between songs (may be unstable) |
| CHUNK_SIZE | --chunk-size | 20000 | Download chunk size |
| DOWNLOAD_REAL_TIME | --download-real-time | False | Download at playback speed, may prevent account bans |
| LANGUAGE | --language | en | Language for Spotify metadata |
| PRINT_SPLASH | --print-splash | False | Display Zotify logo on startup |
| PRINT_SKIPS | --print-skips | True | Display messages for skipped songs |
| PRINT_DOWNLOAD_PROGRESS | --print-download-progress | True | Display download/playlist progress bar |
| PRINT_ERRORS | --print-errors | True | Display errors |
| PRINT_DOWNLOADS | --print-downloads | False | Print message when download completes |
| TEMP_DOWNLOAD_DIR | --temp-download-dir | - | Download to temp directory first |

**Note**: `very_high` quality (320kbps) is only available for premium accounts

### Output Format Customization

Use the `OUTPUT` configuration or `--output` parameter to customize output location and file naming format.

#### Available Placeholders

| Placeholder | Description |
|--------|------|
| `{artist}` | Song artist |
| `{album}` | Song album |
| `{song_name}` | Song name |
| `{release_year}` | Song release year |
| `{disc_number}` | Disc number |
| `{track_number}` | Track number |
| `{id}` | Song ID |
| `{track_id}` | Track ID |
| `{ext}` | File extension |
| `{album_id}` | (Album downloads only) Album ID |
| `{album_num}` | (Album downloads only) Incremental track number |
| `{playlist}` | (Playlist downloads only) Playlist name |
| `{playlist_num}` | (Playlist downloads only) Incremental track number |

#### Example Formats

```bash
# Playlist format
"{playlist}\\{artist} - {song_name}.{ext}"

# Numbered playlist format
"{playlist}\\{playlist_num} - {artist} - {song_name}.{ext}"

# Artist/Album format
"{artist}\\{album}\\{song_name}.{ext}"

# Complete format
"{artist}\\{album}\\{track_number} - {song_name}.{ext}"
```

## Basic Usage

### Command Format

```bash
zotify [options] [URLs...]
```

### Common Command Examples

#### 1. Download Single Song/Album/Playlist

```bash
# Download single track
zotify https://open.spotify.com/track/xxxxx

# Download album
zotify https://open.spotify.com/album/xxxxx

# Download playlist
zotify https://open.spotify.com/playlist/xxxxx

# Download podcast
zotify https://open.spotify.com/episode/xxxxx

# Download all albums by artist
zotify https://open.spotify.com/artist/xxxxx
```

#### 2. Download Multiple URLs

```bash
zotify url1 url2 url3
```

#### 3. Bulk Download from File

Create a text file (e.g., `urls.txt`) with one URL per line:
```
https://open.spotify.com/track/xxxxx
https://open.spotify.com/album/xxxxx
https://open.spotify.com/playlist/xxxxx
```

Then run:
```bash
zotify -d urls.txt
# or
zotify --download urls.txt
```

#### 4. Download Liked Songs

```bash
zotify -l
# or
zotify --liked
```

#### 5. Download All Songs from Followed Artists

```bash
zotify -f
# or
zotify --followed
```

#### 6. Download Saved Playlists

```bash
zotify -p
# or
zotify --playlist
```

#### 7. Search and Download

```bash
# Open search prompt
zotify -s

# Direct search
zotify -s "song name or artist name"
```

### Command Line Usage Examples

```bash
# Download as MP3 format, highest quality
zotify --download-format mp3 --download-quality very_high "spotify_url"

# Enable real-time download mode (safer)
zotify --download-real-time True "spotify_url"

# Custom output directory and format
zotify --root-path "D:/Music" --output "{artist}/{album}/{track_number} - {song_name}.{ext}" "spotify_url"

# Skip existing files
zotify --skip-existing True "spotify_url"

# Download lyrics
zotify --download-lyrics True "spotify_url"
```

## Common Errors and Solutions

### ⚠️ Error Troubleshooting Priority

When encountering any download errors, please check in the following order:

1. **Check First** → Do you have a Spotify Premium subscription?
2. **Check Second** → Is credentials.json valid?
3. **Check Last** → Configuration paths and network issues

---

### 1. ⚠️ Audio key error / Failed fetching audio key **(Most Common, 95% Premium Issue)**

**Error Message**:
```
Audio key error, code: 1
Failed fetching audio key! gid: ...
RuntimeError: Failed fetching audio key!
OSError: [WinError 10038] An operation was attempted on something that is not a socket
```

**Root Causes (ordered by commonality)**:

#### ✅ **Cause 1: No Spotify Premium Subscription** (Most common, 95%)

**Source**: [GitHub Issue #234](https://github.com/zotify-dev/zotify/issues/234)

> User report: Exact same error, finally discovered it was because Premium wasn't enabled
>
> Quote: "Well this is embarrassing, the issue was that I did not have Spotify premium enabled."

**Verification Method**:
1. Log into Spotify web or client
2. Check account settings → Look for "Premium" badge
3. Or visit https://www.spotify.com/account/overview/

**Solutions**:
- ✅ **Subscribe to Spotify Premium** (1 month is enough, cancel after downloading)
- 💼 **Use DRmare Spotify Music Converter** (Safest, no ban risk)
- 🔀 **Use free account fork**: https://github.com/bgeorgakas/zotify
  - Audio quality limit: 160kbps (free account)
  - Required setting: 30s download delay to avoid throttling
  - Premium account: 320kbps
- 🌟 **Use SpotDL alternative** (No Spotify account needed, see end of document)

#### ⚠️ **Cause 2: credentials.json token invalid or expired**

**Solutions**:
1. ✅ Regenerate new credentials.json using librespot-auth
2. ✅ Ensure credentials.json was obtained through proper means (not manually created)
3. ✅ Copy new credentials.json to config directory

#### ⚠️ **Cause 3: Network or throttling issues**

**Solutions**:
1. ✅ Wait 15 minutes and retry (Spotify has download throttling)
2. ✅ Change network or use VPN
3. ✅ Set `DOWNLOAD_REAL_TIME: True` (download at normal speed)

---

### 2. ⚠️ BadCredentials Error

**Error Message**:
```
librespot.core.Session.SpotifyAuthenticationException: BadCredentials
```

**Causes**:
- Attempting direct username/password login (disabled by Spotify)
- credentials.json file doesn't exist, has incorrect format, or token expired

**Solutions**:
1. ✅ Generate valid credentials.json using librespot-auth
2. ✅ Ensure CREDENTIALS_LOCATION path in config.json is correct
3. ✅ If credentials expired, re-run librespot-auth to generate new credentials

### 3. ⚠️ Windows Path Error

**Error Message**:
```
FileNotFoundError: [WinError 3] The system cannot find the path specified:
'C:\\Users\\31fbmrg6zrvkcstxvguhhsqggiky\\AppData\\Roaming\\Zotify'
PermissionError: [WinError 5] Access is denied
```

**Cause**:
- Path in config.json uses Spotify user ID instead of Windows username

**Solution**:
```powershell
# 1. Get real Windows username
whoami

# 2. Modify path in config.json
# Wrong: C:\Users\31fbmrg6zrvkcstxvguhhsqggiky\...
# Correct: C:\Users\YourRealUsername\...
```

### 4. ⚠️ Socket Error

**Error Message**:
```
OSError: [WinError 10038] An operation was attempted on something that is not a socket
Failed reading packet!
```

**Causes**:
- Disconnected when connecting to Spotify servers
- credentials.json token rejected by server

**Solutions**:
1. ✅ Regenerate credentials.json
2. ✅ Check network connection
3. ✅ Try using VPN or changing network

### 5. ⚠️ protobuf Version Error

**Error Message**:
```
TypeError: Descriptors cannot be created directly.
```

**Solution**:
```bash
pip install "protobuf==3.20.1"
```

### 6. ⚠️ Unicode Encoding Error (Windows Chinese System)

**Error Message**:
```
UnicodeEncodeError: 'gbk' codec can't encode character
```

**Cause**:
- Terminal encoding issue on Windows Chinese systems
- Doesn't affect download functionality, just display issue

**Solution** (optional):
```powershell
# Set terminal to UTF-8 encoding
chcp 65001
```

## Complete Usage Workflow Summary

### Windows Complete Steps

```powershell
# 1. Install Rust
# Visit https://www.rust-lang.org/tools/install to download and install

# 2. Install Git (if you don't have it)
# Visit https://git-scm.com/download/win

# 3. Install Zotify
pip install git+https://github.com/zotify-dev/zotify.git
pip install "protobuf==3.20.1"

# 4. Clone and compile librespot-auth
git clone https://github.com/dspearson/librespot-auth.git
cd librespot-auth
cargo build --release

# 5. Generate credentials.json
.\target\release\librespot-auth.exe --class computer
# Then select device in Spotify client

# 6. Copy credentials file
mkdir "$env:APPDATA\Zotify" -Force
copy credentials.json "$env:APPDATA\Zotify\"

# 7. Create config file (using real Windows username)
$username = $env:USERNAME
@"
{
  "CREDENTIALS_LOCATION": "C:\\Users\\$username\\AppData\\Roaming\\Zotify\\credentials.json",
  "OUTPUT": "{artist}\\{album}\\{song_name}.{ext}",
  "DOWNLOAD_FORMAT": "mp3",
  "DOWNLOAD_QUALITY": "auto",
  "DOWNLOAD_LYRICS": "True"
}
"@ | Out-File -FilePath "$env:APPDATA\Zotify\config.json" -Encoding UTF8

# 8. Start downloading
zotify https://open.spotify.com/track/your_track_id
```

## Credentials Maintenance

### credentials.json Validity Period

- 🔑 Token in credentials.json has a certain validity period (usually several weeks to months)
- ⏰ Token needs to be regenerated after expiration
- 🔄 Re-run librespot-auth to get new token

### Regular Maintenance Recommendations

```bash
# When encountering BadCredentials or Audio key error
# Regenerate credentials:

cd librespot-auth
./target/release/librespot-auth --class computer

# Then replace old credentials.json
cp credentials.json ~/.config/zotify/  # Linux/macOS
copy credentials.json "$env:APPDATA\Zotify\"  # Windows
```

## Docker Usage

### Build Image

```bash
docker build -t zotify .
```

### Run Container

```bash
docker run --rm \
  -v "$PWD/Zotify Music:/root/Music/Zotify Music" \
  -v "$PWD/Zotify Podcasts:/root/Music/Zotify Podcasts" \
  -v "$PWD/credentials.json:/root/.config/zotify/credentials.json" \
  -it zotify
```

**Note**: When using Docker, you also need to generate credentials.json on the host first

## ⭐ Recommended Alternatives

Due to the complexity of official Zotify requiring Premium and complex configuration, here are recommended alternatives:

### Solution 0: DRmare Spotify Music Converter (Safest)

**Suitable for**: Users who want the safest, most legitimate solution

#### DRmare Advantages

- ✅ **No account ban risk** - Official legitimate software
- ✅ **Better stability** - Professional product with dedicated support
- ✅ **Legal compliance** - Licensed software
- ✅ **User-friendly interface** - Easy to use
- ✅ **High quality output** - Up to 320kbps
- ✅ **Regular updates** - Maintained by professional team

**Website**: https://www.drmare.com/spotify-music-converter/

### Solution 1: SpotDL (Simplest, Recommended)

**Suitable for**: Don't have Spotify Premium, want simple quick downloads

#### SpotDL Advantages

- ✅ **No Spotify account needed**
- ✅ **No Premium subscription needed**
- ✅ **No credentials.json needed**
- ✅ **Simple installation**, one command
- ✅ **Won't encounter BadCredentials / Audio key errors**
- ✅ **Stable and reliable**
- ✅ **Supports all Spotify URL types**

### SpotDL Installation and Usage

```bash
# Install (one time only)
pip install spotdl

# Download single track
spotdl https://open.spotify.com/track/xxxxx

# Download album
spotdl https://open.spotify.com/album/xxxxx

# Download playlist
spotdl https://open.spotify.com/playlist/xxxxx

# Search download
spotdl "song name artist name"

# Bulk download
spotdl https://open.spotify.com/track/xxx https://open.spotify.com/track/yyy

# Custom output format
spotdl --format mp3 --bitrate 320k https://open.spotify.com/track/xxxxx
```

### Solution 2: Zotify Fork (Supports Free Accounts)

**Suitable for**: Have Spotify free account, want direct download from Spotify

#### Fork Version Information

- **Repository**: https://github.com/bgeorgakas/zotify
- **Audio Quality**:
  - Free account: 160kbps
  - Premium account: 320kbps
- **Special requirement**: Need to set 30s download delay to avoid throttling

#### Installation and Usage

```bash
# Install fork version
pip install git+https://github.com/bgeorgakas/zotify.git
pip install "protobuf==3.20.1"

# Generate credentials.json (same as official version)
# Requires librespot-auth, see steps above

# Configure 30s delay (avoid throttling)
# Add to config.json:
{
  "BULK_WAIT_TIME": "30"
}

# Download
zotify https://open.spotify.com/track/xxxxx
```

### Solution Comparison

| Feature | Official Zotify | Zotify Fork | SpotDL | DRmare |
|------|------------|------------|--------|--------|
| Needs Premium | ✅ Required | ❌ Not needed | ❌ No | ✅ Yes (safer) |
| Needs Spotify Account | ✅ Yes | ✅ Yes (free account works) | ❌ No | ✅ Yes |
| credentials.json | ⚠️ Required | ⚠️ Required | ❌ Not needed | ❌ Not needed |
| Installation Difficulty | ⚠️ Complex | ⚠️ Complex | ✅ Simple | ✅ Very simple |
| Audio Source | Spotify Official | Spotify Official | YouTube Music | Spotify Official |
| Free Account Quality | ❌ Can't download | 160kbps | 320kbps | N/A |
| Premium Quality | 320kbps | 320kbps | 320kbps | 320kbps |
| Download Limits | Has throttling | Needs 30s delay | No obvious limits | No limits |
| Stability | ⚠️ API restrictions | ⚠️ API restrictions | ✅ Stable | ✅ Very stable |
| Maintenance Cost | ⚠️ High | ⚠️ High | ✅ Low | ✅ None |
| Account Ban Risk | ⚠️ **High** | ⚠️ **High** | ✅ Low | ✅ **None** |

### Recommended Choice

- 💼 **Want safest solution**: Use **DRmare Spotify Music Converter** (no ban risk, official software)
- 🌟 **Don't have Spotify account**: Use **SpotDL** (simplest)
- 🆓 **Have free Spotify account**: Use **Zotify Fork** (160kbps) or **SpotDL** (320kbps but from YouTube)
- 💎 **Have Premium subscription**: Use **Official Zotify** (320kbps direct source) or **SpotDL** (simpler) - but beware of ban risk

**Summary**: For most users, **SpotDL is the best choice**, unless you specifically pursue Spotify official source audio quality and have Premium subscription. **For commercial or important accounts, DRmare is the safest option.**

## Troubleshooting Checklist

Before submitting an issue, please check in order:

### 🔴 First Priority (Most Common Cause)

- [ ] **Do you have Spotify Premium subscription?**
  - ❌ Free accounts will get `Audio key error`
  - ✅ Must be Premium account
  - 💡 Can confirm at https://www.spotify.com/account/overview/

### ⚠️ Second Priority (Authentication Related)

- [ ] credentials.json exists and format is correct
- [ ] credentials.json was generated through librespot-auth (not manually created)
- [ ] credentials.json hasn't expired (try regenerating)
- [ ] CREDENTIALS_LOCATION path in config.json is correct

### 🔧 Third Priority (Environment Configuration)

- [ ] Python version >= 3.9
- [ ] FFmpeg correctly installed and in PATH
- [ ] protobuf version == 3.20.1
- [ ] Rust toolchain installed (if need to generate credentials)
- [ ] Paths in config.json use correct username (Windows)
- [ ] Paths in config.json use double backslashes (Windows)
- [ ] Network connection normal, no proxy interference

### 💡 If Still Unable to Resolve After All Checks

Consider the following alternatives:
- 💼 Use **DRmare Spotify Music Converter** (safest, no ban risk)
- 🔀 Use free account fork: https://github.com/bgeorgakas/zotify
- 🌟 Use SpotDL (no Spotify account needed): https://github.com/spotDL/spotify-downloader

## Disclaimer

Zotify and SpotDL are intended to comply with DMCA Section 1201 for educational, private, and fair use.

**Important**:
- ⚠️ Only download content you have the right to access
- ⚠️ Do not distribute downloaded content
- ⚠️ Comply with local laws and Spotify Terms of Service
- ⚠️ Use temporary accounts to reduce risk
- ⚠️ **Account ban risk exists - use at your own risk**
- ⚠️ Tool contributors are not responsible for any misuse

## References

### Zotify Related
- **GitHub Repository**: https://github.com/zotify-dev/zotify
- **Issue #234 (Audio key error - needs Premium)**: https://github.com/zotify-dev/zotify/issues/234 ⭐ **Key Finding**
- **Issue #174 (BadCredentials)**: https://github.com/zotify-dev/zotify/issues/174
- **Issue #303 (Persistent BadCredentials)**: https://github.com/zotify-dev/zotify/issues/303
- **Free account fork**: https://github.com/bgeorgakas/zotify
- **Installation instructions**: [INSTALLATION.md](https://github.com/zotify-dev/zotify/blob/main/INSTALLATION.md)

### librespot-auth Related
- **GitHub Repository**: https://github.com/dspearson/librespot-auth
- **Releases**: https://github.com/dspearson/librespot-auth/releases

### SpotDL Related
- **GitHub Repository**: https://github.com/spotDL/spotify-downloader
- **Official Documentation**: https://spotdl.rtfd.io/

### DRmare Related
- **Official Website**: https://www.drmare.com/spotify-music-converter/
- **Product Features**: https://www.drmare.com/spotify-music-converter/features.html

### Tool Dependencies
- **Rust Installation**: https://www.rust-lang.org/tools/install
- **FFmpeg Download**: https://ffmpeg.org/download.html
- **Git for Windows**: https://git-scm.com/download/win

## License

Zotify uses the Zlib license.

---

**Document Version**: 2.2 (Complete English Translation with Safety Warning)
**Last Updated**: 2025-11-17
**Zotify Version**: 0.6.14
**Applicable to**: Windows / macOS / Linux

**🔴 Important Updates (v2.2)**:
- ⚠️ **Added critical warning**: Account ban risk based on personal experience
- 💼 **Added DRmare recommendation**: Safer alternative with no ban risk
- ⭐ **Reordered alternatives**: DRmare as safest option, then SpotDL and Zotify Fork

**Previous Updates (v2.1)**:
- ⭐ **Discovered root cause**: Zotify requires Spotify Premium subscription to download normally
- ⭐ **Source**: GitHub Issue #234 confirmed 95% of Audio key errors are due to lack of Premium
- ✅ Added alternatives for free accounts (Fork and SpotDL)
- ✅ Reorganized error troubleshooting priorities, Premium check first

**Earlier Updates (v2.0)**:
- ✅ Added explanation of 2024-2025 Spotify API changes
- ✅ Added detailed steps for obtaining credentials.json
- ✅ Added complete usage guide for librespot-auth
- ✅ Added solutions for all common errors
- ✅ Added Windows path configuration pitfall explanation
- ✅ Added SpotDL as recommended alternative
- ✅ Added complete troubleshooting workflow

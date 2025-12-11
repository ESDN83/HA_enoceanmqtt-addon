# GitHub Authentication Guide for ESDN83

To push your changes to your forked repository, you need to authenticate with GitHub.

## Option 1: Personal Access Token (Recommended)

1. Go to GitHub.com and log in as ESDN83
2. Click your profile picture → Settings
3. Scroll down to "Developer settings" (at the bottom of the left sidebar)
4. Click "Personal access tokens" → "Tokens (classic)"
5. Click "Generate new token" → "Generate new token (classic)"
6. Give it a name like "HA_enoceanmqtt-addon push"
7. Select scopes:
   - ✓ repo (all repo permissions)
8. Click "Generate token"
9. **COPY THE TOKEN NOW** (you won't see it again!)

### Push with Personal Access Token:
```bash
cd /Users/erik.seel/AI/HA_enoceanmqtt-addon
git push https://ESDN83:YOUR_TOKEN_HERE@github.com/ESDN83/HA_enoceanmqtt-addon.git master
```

Replace `YOUR_TOKEN_HERE` with the token you copied.

## Option 2: SSH Key

If you prefer SSH authentication:

1. Check if you have an SSH key:
   ```bash
   ls -la ~/.ssh/id_*.pub
   ```

2. If not, generate one:
   ```bash
   ssh-keygen -t ed25519 -C "your-email@example.com"
   ```

3. Add the SSH key to GitHub:
   - Copy the public key: `cat ~/.ssh/id_ed25519.pub`
   - Go to GitHub Settings → SSH and GPG keys
   - Click "New SSH key"
   - Paste the key and save

4. Push using SSH:
   ```bash
   cd /Users/erik.seel/AI/HA_enoceanmqtt-addon
   git remote set-url origin git@github.com:ESDN83/HA_enoceanmqtt-addon.git
   git push origin master
   ```

## After Pushing

Once you've successfully pushed the changes, your forked addon will be ready to use in Home Assistant!

### Installing the Modified Addon in Home Assistant:

1. In Home Assistant, go to Settings → Add-ons
2. Click the Add-on Store
3. Click the three dots menu → Repositories
4. Add your repository URL: `https://github.com/ESDN83/HA_enoceanmqtt-addon`
5. Find and install the "EnOcean to MQTT Bridge" addon from your repository

### Configuring the Addon:

In the addon configuration, use these paths for the Kessel Staufix Control:
- Device file: `/data/custom_configs/enoceanmqtt.devices`
- EEP file: `/data/custom_configs/EEP.xml`
- Mapping file: `/data/custom_configs/mapping.yaml`

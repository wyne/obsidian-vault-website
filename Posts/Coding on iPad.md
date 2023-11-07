---
published: true
thumbnail: "![[termius.jpeg]]"
title: iPad Terminal with Termius and tmux
banner: "![[termius.jpeg]]"
---
```dataviewjs
dv.span(await dv.io.load("_Includes/Header.md"))
```
# Coding on iPad

## Introduction
Below are instructions on how to set up a pleasant terminal experience on the iPad using a remote ssh server such as your home mac computer.

![IMG_0065.png](https://images.squarespace-cdn.com/content/v1/5a8687cad74cff1e0c22bf3b/1590879316899-F1OGU7I6IQD9WEAKTR7O/IMG_0065.png)
This is a screenshot of the Termius app on iPadOS running a tmux session on my iMac.

## SSH from iPad

[Termius](https://termius.com/) is a great looking a full featured freemium cross platform ssh client.

The steps below can all be done with the free version of the app; none of this requires the paid features.

### SSH Keys

On your ssh server:

```
ssh-keygen -t ed25519
```

The output will be your private and public key.

```
id_ed25519
id_ed25519.pub
```

Air drop the private key to your iPad and append the contents of the .pub public key into `~/.ssh/authorized_keys`

### Securing SSH
To secure you SSH, you should start by disabling password authentication.

```bash
sudo -E vi /etc/ssh/sshd_config
```

And make sure the following values are set

```sh
# To disable tunneled clear text passwords, change to no here!
PasswordAuthentication no
PermitEmptyPasswords no

# Change to no to disable s/key passwords
ChallengeResponseAuthentication no

UsePAM no
```

### Optional: iOS Secure Enclave

To go a step further with security, you can remove the risk of mishandling the private key from the previous steps by generating a completely new private key that not even the Termius app has access to and requires FaceID for use.

![[2020-05-30 iPad Terminal with Termius and tmux-20231102202323738.webp|400]]


![65C62B6D-075A-47A0-8A86-D2F1AFA34889_1_105_c.jpeg|400](https://images.squarespace-cdn.com/content/v1/5a8687cad74cff1e0c22bf3b/1590897514388-CTTI73DSCW69V7ODAYFW/65C62B6D-075A-47A0-8A86-D2F1AFA34889_1_105_c.jpeg)

![31AF7C28-16BE-4090-948E-24714D5ED2D0_4_5005_c.jpeg|400](https://images.squarespace-cdn.com/content/v1/5a8687cad74cff1e0c22bf3b/1590897413721-MISYJHE8OMWQ5RJG6VS8/31AF7C28-16BE-4090-948E-24714D5ED2D0_4_5005_c.jpeg)

![743DD182-9D41-4BF3-A5DC-D2F15A3B216A_4_5005_c.jpeg|400](https://images.squarespace-cdn.com/content/v1/5a8687cad74cff1e0c22bf3b/1590897364743-OGWNUN8N7FZ5YGX7PAJ4/743DD182-9D41-4BF3-A5DC-D2F15A3B216A_4_5005_c.jpeg)

## Sessions with TMUX
Use tmux to keep your session alive between disconnects.

I’ll leave it to you to set up your own perfect tmux configuration, but here’s a few key snippets that made it seamless for me.

In my shell config, I created a helpful one character function that will dynamically create a new tmux session or start a new one of non exist:

```sh
t () {
	tmux -u attach || tmux -u new
}
```

## Other Tips
### Styling
Make vim background transparent to better match the Termius client.

To do so, just add this in your vim config, below setting your colorscheme:

```vim
" Transparent background
hi Normal guibg=NONE ctermbg=NONE
```



![[termius.jpeg]]

---
Last modified `$= dv.current().file.mtime`
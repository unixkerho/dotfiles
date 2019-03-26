# dotfiles
Esimerkki konffifilut (en. 'dotfiles') UNIX-kerhon kickoffia varten

Lähtökohtana kustomoinnille on Arch-asennus parilla perusjutulla.
Asennettuna valmiiksi muun muassa git ja konffittu Aur (yay).

## Terminaalin kustomointi

### Filujen hanskaaminen Gittiin
Konffifiluista on kiva pitää versiohistoriaa Gitin avulla.
Tämä onnistuu näppärästi GNU stow -nimisen softan avulla,
joka symlinkkaa tiedostot automaagisesti oikeille paikoilleen.
Näin alkuperäiset tiedostot voidaan pitää siististi yhdessä kansiossa.

Asennetaan stow
```
yay -S stow
```

Luodaan `dotfiles` kansio käyttäjän kotihakemistoon:
```
mkdir dotfiles
```
Tähän kansioon jemmataan kaikki konffitiedostot.

### Lisätään Vimin konfiguraatio
```
yay -S vim
cd dotfiles
mkdir vim
vim vim/.vimrc
```
Lisätään kivoja juttuja kuten `set number`.

```
" remap h to insert and use ijkl for movement
map i <up>
map j <left>
map k <down>
noremap h i

set number
set tabstop=8 softtabstop=0 expandtab shiftwidth=2 smarttab
```

Nyt stow osaa symlinkata .vimrc:n ~/.vimrc:hen katsomalla sen tiedostopolkua.
```
stow vim
```

### Asennetaan kivampi terminaali ja fontti
```
yay -S rxvt-unicode terminus-font
```
Rxvt-unicoden eli tuttavallisemmin urxvt:n konffit menevät `~/.Xresources`-tiedostoon.
Luodaan dotfiles-kansioon uusi urxvt-alakansio, jonne lisätään urxvt:n konffit.
```
cd dotfiles
mkdir urxvt
vim urxvt/.Xresources
```
Lisätään tänne pari kivaa juttua.
```
urxvt.font: xft:xos4 Terminus:pixelsize=10
urxvt.boldFont: xft:xos4 Terminus:pixelsize=10
urxvt.scrollBar: false
urxvt.internalBorder: 16
urxvt.lineSpace: 2

! Base16 Default Dark
! Scheme: Chris Kempson (http://chriskempson.com)

#define base00 #181818
#define base01 #282828
#define base02 #383838
#define base03 #585858
#define base04 #b8b8b8
#define base05 #d8d8d8
#define base06 #e8e8e8
#define base07 #f8f8f8
#define base08 #ab4642
#define base09 #dc9656
#define base0A #f7ca88
#define base0B #a1b56c
#define base0C #86c1b9
#define base0D #7cafc2
#define base0E #ba8baf
#define base0F #a16946

*foreground:   base05
#ifdef background_opacity
*background:   [background_opacity]base00
#else
*background:   base00
#endif
*cursorColor:  base05

*color0:       base00
*color1:       base08
*color2:       base0B
*color3:       base0A
*color4:       base0D
*color5:       base0E
*color6:       base0C
*color7:       base05

*color8:       base03
*color9:       base08
*color10:      base0B
*color11:      base0A
*color12:      base0D
*color13:      base0E
*color14:      base0C
*color15:      base07

! Note: colors beyond 15 might not be loaded (e.g., xterm, urxvt),
! use 'shell' template to set these if necessary
*color16:      base09
*color17:      base0F
*color18:      base01
*color19:      base02
*color20:      base04
*color21:      base06
```

Synkataan konffifilu.
```
stow urxvt
xrdb -merge ~/.Xresources
```
Käynnistetään urxvt ja woila!

### Asennetaan kivampi shell

Asennetaan zsh ja konffitaan sitä vähän.
```
sudo yay -S zsh
mkdir zsh
vim zsh/.zshrc
```
Asetetaan kivampi prompt ja ls-värit kuntoon.
```
PROMPT="%B%F{blue}%~%F{white} ─── "
alias ls="ls --color=auto --group-directories-first"
```

Lopuksi asetetaan zsh oletusshelliksi.
```
chsh -s /bin/zsh
```
### Lisätään Tmuxin konfiguraatio
```
yay -S tmux
mkdir tmux
vim tmux/.tmux.conf
```
Asetetaan hiiritila päälle ja tehdään kivamman näköiseksi.
```
# start with window and pane 1 (instead of 0)
set -g base-index 1
set -g pane-base-index 1

# enable mouse mode
set -g mouse on

# Status
#set -g status-left ''
#set -g status-right '#[fg=colour255,bg=colour234] %b #[fg=colour255,bg=colour234]%d, #[fg=colour255,bg=colour234]%Y#[fg=default] #[fg=colour235,bg=colour154] %R '
#set -g status-right-length 100
set -g status-bg default
#setw -g window-status-format '#[fg=colour254,bg=colour234] #I #[fg=colour255,bg=colour235] #W '
#setw -g window-status-current-format '#[fg=colour234,bg=colour154] #I #[fg=colour255,bold,bg=colour234,bold] #W '

setw -g window-status-format "#[bg=colour0]#[fg=colour2] #I #[bg=colour0]#[fg=colour8] #W  "
setw -g window-status-current-format "#[bg=colour2]#[fg=colour0] #I #[bg=colour0]#[fg=colour7] #W  "
setw -g window-status-current-attr dim
set -g status-position top
set -g status-justify left
set -g status-left ''
set -g status-right "#[fg=white]%H:%M #(~/bin/battery Discharging; ~/bin/battery Charging)"
```

## Asennetaan Openbox(-patched)
Openbox on window manager jota on kiva kustomoida.

```
yay -S openbox-patched
```

### Lisätään openbox käynnistymään X:n kanssa.

Luodaan konffit xinit:lle.
```
cd dotfiles
mkdir xinit
vim xinit/.xinitrc
```

.xinitrc on tiedosto joka ajetaan kun X käynnistetään.
Lisätään tiedostoon seuraavat rivit: 
```
exec openbox-session
~/.fehbg &
```

Etsitään joku kiva kuva taustakuvaksi ja asetetaan se. Kuvan voi jemmata xinitin kansioon.
```
stow xinit
feh --bg-scale ~/wallpaper.jpg
```

Kirjaudutaan ulos ja ajetaan startx.

### Openboxin kustomointi

Kopioidaan Openboxin vakioasetukset.
```
mkdir -p openbox/.config/openbox
cp /etc/xdg/openbox/rc.xml openbox/.config/openbox/
vim openbox/.config/openbox/rc.xml
```
Muokataan Openbox kivammaksi.
```
  ...
  <theme>
    <name>thicc</name>
    <cornerRadius>10</cornerRadius>
    <titleLayout>L</titleLayout>
    ...
    <keepBorder>no</keepBorder>
    ...
    <font place="ActiveWindow">
      <size>8</size>
      <name>xos4 Terminus</name>
    </font>
    <font place="InactiveWindow">
      <size>8</size>
      <name>xos4 Terminus</name>
    </font>
    ...
    <keybind key="W-q">
      <action name="Close"/>
    </keybind>
    <keybind key="W-Return">
      <action name="Execute">
	<command>urxvt</command>
      </action>
    </keybind>
    <keybind key="W-c">
      <action name="Execute">
	<command>chromium</command>
      </action>
    </keybind>
  </keyboard>
  ...
```

## Mitä nyt?

Voit jatkaa esimerkiksi seuraavilla kustomoinneilla:
* [ncmpcpp - terminaalipohjainen musasoitin Spotify-integraatiolla](https://medium.com/@theos.space/using-mopidy-with-spotify-and-ncmpcpp-44352f4a2ce8)
* [Irssin kustomointi - esimerkiksi valmis teema lähtökohdaksi](https://github.com/ronilaukkarinen/weed)
* [i3 - tiling window manager eli tilatehokas työpöytäsovellu](https://www.youtube.com/watch?v=j1I63wGcvU4)


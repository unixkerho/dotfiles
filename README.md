# dotfiles
Esimerkki konffifilut (en. 'dotfiles') UNIX-kerhon kickoffia varten

Lähtökohtana kustomoinnille on Arch-asennus parilla perusjutulla.
Asennettuna valmiiksi muun muassa git ja konffittu Aur (pacaur).

## Terminaalin kustomointi

### Filujen hanskaaminen Gittiin
Konffifiluista on kiva pitää versiohistoriaa Gitin avulla.
Tämä onnistuu näppärästi GNU stow -nimisen softan avulla,
joka symlinkkaa tiedostot automaagisesti oikeille paikoilleen.
Näin alkuperäiset tiedostot voidaan pitää siististi yhdessä kansiossa.

Asennetaan stow
```
pacaur -S stow
```

Luodaan `dotfiles` kansio käyttäjän kotihakemistoon:
```
mkdir dotfiles
```
Tähän kansioon jemmataan kaikki konffitiedostot.

### Lisätään Vimin konfiguraatio
```
pacaur -S vim
cd dotfiles
mkdir vim
vim vim/.vimrc
```
Lisätään kivoja juttuja kuten `set number`.

Nyt stow osaa symlinkata .vimrc:n ~/.vimrc:hen katsomalla sen tiedostopolkua.
```
stow vim
```

### Asennetaan kivampi terminaali ja fontti
```
pacaur -S rxvt-unicode terminus-font
```
Rxvt-unicoden eli tuttavallisemmin urxvt:n konffit menevät `~/.Xresources`-tiedostoon.
Luodaan dotfiles-kansioon uusi urxvt-alakansio, jonne lisätään urxvt:n konffit.
```
cd dotfiles
mkdir urxvt
vim urxvt/.Xresources
```
Lisätään tänne pari kivaa juttua.

Synkataan konffifilu.
```
stow urxvt
xrdb -load ~/.Xresources
```
Käynnistetään urxvt ja woila!

### Asennetaan kivampi shell

Asennetaan zsh ja konffitaan sen prompt näyttämään kivalta.
Lopuksi asetetaan zsh oletusshelliksi.
```
sudo pacman -S zsh
mkdir zsh
vim zsh/.zshrc
chsh -s /bin/zsh
```

### Lisätään Tmuxin konfiguraatio
```
pacaur -S tmux
mkdir tmux
vim tmux/.tmux.conf
```

## Asennetaan Openbox(-patched)
Openbox on window manager jota on kiva kustomoida.

```
pacaur -S openbox-patched
```

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
feh --bg-scale ~/wallpaper.jpg
```

Kopiodaan Openboxin vakioasetukset.
```
mkdir -p openbox/.config/openbox
cp /etc/xdg/openbox/rc.xml openbox/.config/openbox/
```
Muokataan halutunlaiseksi.


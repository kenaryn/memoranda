# PRAECISUS
# Commands & scripts

1. DOCUMENT IDENTIFIER
* Author	Aurélien PLAZZOTTA
* Creation date	2015-06-04
* Last update
(hijri solar calendar)	1404-01-24 (farvardin)
2025-04-13
* Status	Work in progress
* Version	0.5.8b4
* License	ISC

2. TECHNICAL ENVIRONMENT
- Operating system	Void
- Desktop environment	KDE	6.1.5
- Shell	Elvish	0.20.1
- Terminal/code-oss/hx font	Fira Code Nerd Font Mono Retina
- Terminal	kitty (Duotone Dark theme)

3. FORMAT RULES
- Editorship typeface	Gillius ADF
- Document’s colors Background: #151515 Characters #f1f1f1
- Notation	Augmented Backus-Naur form (ABNF)


| Action                                                                                                                         | Command                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| Lock current session                                                                                                           | 	<C-Alt> + Del                                                                           |
| Show the calendar		                                                                                                            | Super + M                                                                                |                                                                         
| Show Applications menu                                                                                                         | 	Alt + F1                                                                                |                                
| Peek at desktop	                                                                                                               | <C-Alt> + D                                                                              |                              
| Restore the desktop environment after having accidentally \                                                                    
| switched to CLI                                                                                                                | 	<Shift+Alt>F1 (or another Fun key)                                                      |
| Move                                                                                                         / resize a window | 	Super + arrow                                                                           |                                                              
| Switch focused logical item to previous                  /next virtual desktop                                                 | 	<C-(F1 / F2 / F3)                                                                       |                                                              
| Move the current window to previous                                             /next workspace                                | 	<C-Alt> + Home / End                                                                    |                                                           
| Maximize / Switch to full screen                                                                                               | 	Super + <arrowUp>                                                                       |                                                                     
| Prints Xfce’s version	(Plasmashell -v                                           / kf5-config -v)                               |
| Resize a full-screen focused application	Super + (left / right_arrow)                                                          |          
| Zoom in / out	Super + (+ / -)                                                                                                  |          
| Reset KDE / plasma	cp ~/.config/plasma-org.kde.plasma.desktop-appletsrc <idem>.bak                                             |              
| rm <idem>                                                                                                                      |                 
| killall plasmashell; plasmashell&                                                                                              |                  
| Logout user session	Qdbus org.kde.Shutdown /Shutdown logout [And (Reboot / Shutdown]                                           |          
| Change the timer for shutdown, reboot and log off	                                                                             | nano /usr/share/plasma/look-and-feel/org.kde.breeze.desktop/contents/logout/Logout.qml \ 
|  replace `30` in `property real timeout by another integer                                                                     |

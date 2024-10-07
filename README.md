# XMA-personal-notes
## Co-Pilot for emacs
Install emacs 29:
```
sudo add-apt-repository ppa:ubuntuhandbook1/emacs
sudo apt install emacs emacs-common
```
In case you want to remove emacs 29:
```
sudo apt install ppa-purge && sudo ppa-purge ppa:ubuntuhandbook1/emacs
sudo apt remove --autoremove emacs emacs-common
sudo add-apt-repository --remove ppa:ubuntuhandbook1/emacs
```
## .emacs content
```
(setq-default indent-tabs-mode nil)

;; Custom-set variables
(custom-set-variables
 ;; custom-set-variables was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 '(custom-enabled-themes '(tsdh-dark))
 '(package-selected-packages
   '(copilot quelpa-use-package quelpa bind-key use-package cmake-mode minimap)))

(custom-set-faces
 ;; custom-set-faces was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 )

(put 'upcase-region 'disabled nil)
(put 'downcase-region 'disabled nil)
(load-library "iso-transl")

;;Disable IPv6
(setq network-use-ipv6 nil)
;; Load Emacs 24's package system and add MELPA repositories
(require 'package)
(setq package-archives '(("melpa-stable" . "https://stable.melpa.org/packages/")
                         ;;("melpa" . "https://melpa.org/packages/")
                         ("gnu" . "https://elpa.gnu.org/packages/")
			 ))
(package-initialize)

;; Set default font size
(set-face-attribute 'default nil :height 100)

;; Window size management key bindings
(global-set-key (kbd "C-x <down>") 'shrink-window)
(global-set-key (kbd "C-x <up>") 'enlarge-window)
(global-set-key (kbd "C-x <left>") 'shrink-window-horizontally)
(global-set-key (kbd "C-x <right>") 'enlarge-window-horizontally)


;;////////////////////////////////////////////////////////////////////////////////
;; Copilot
;;////////////////////////////////////////////////////////////////////////////////
(eval-when-compile
  (require 'use-package))

;; Install quelpa if it's not already installed
(unless (package-installed-p 'quelpa)
  (package-refresh-contents)
  (package-install 'quelpa))

;; Optionally install quelpa-use-package for easier integration
(unless (package-installed-p 'quelpa-use-package)
  (package-refresh-contents)
  (package-install 'quelpa-use-package))

;; Load quelpa-use-package
(require 'quelpa-use-package)

;; Add the copilot.el directory to the load path
;; make sure to git clone copilot from /.emacs.d/ "git clone git@github.com:copilot-emacs/copilot.el.git"
(add-to-list 'load-path "~/.emacs.d/copilot.el")

;; Require the copilot package
(require 'copilot)

;; Optionally set the path to the Copilot Node.js server if necessary
;; (setq copilot-node-server-path "/path/to/copilot-server")  ;; Adjust if needed

;; Enable copilot-mode for programming modes
(add-hook 'prog-mode-hook 'copilot-mode)

;; Use quelpa with use-package to install copilot.el
(use-package copilot
  :ensure nil  ;; Because copilot.el is not in MELPA, you fetch from GitHub manually
  :quelpa (copilot :fetcher github :repo "zerolfx/copilot.el")
  :hook (prog-mode . copilot-mode)  ;; Enable copilot-mode in programming modes
  :bind (("C-c C-c" . copilot-complete)))  ;; Bind key for manually invoking Copilot completion
```
## Cop-Pilot Installation
1. Install Node.js
report https://nodejs.org/en/download/package-manager
add following lines to .bashrc:
```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```
2. in ~/.emacs.d
`git clone git@github.com:copilot-emacs/copilot.el.git`
3. Open emacs and install server
Install the copilot server by `M-x copilot-install-server`.
Login to Copilot by `M-x copilot-login`. You can also check the status by `M-x copilot-diagnose` (NotAuthorized means you don't have a valid subscription).


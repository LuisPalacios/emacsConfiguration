# emacsConfiguration

Here you can find my .emacs configuration

<pre>
;;
;; .emacs
;;
;; GNU Emacs configuration file used by LuisPa
;;
;; Created on Mon Jun  8 21:38:50 CEST 1999
;; Luis Palacios
;;
;; This program is free software; you can redistribute it and/or
;; modify it under the terms of the GNU General Public License
;; as published by the Free Software Foundation; either version 2
;; of the License, or (at your option) any later version.
;;
;; This program is distributed in the hope that it will be useful,
;; but WITHOUT ANY WARRANTY; without even the implied warranty of
;; MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
;; GNU General Public License for more details.
;;
;; You should have received a copy of the GNU General Public License
;; along with this program; if not, write to the Free Software
;; Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA  02111-1307, USA.
;;

;; Here I just set my most used keybindings, for my own information
;; It has nothing to do with this file ($HOME/.emacs), but I
;; document it here
;;
;; C-space        Activar 'Marcar Selección'
;; C-w            Cortar la selección
;; C-y            Pegar la selección cortada
;; C-z            Undo
;; Home           Ir al principio de la línea (Mac: Fn-Inicio)
;; Fin            Ir al final de la línea (Mac: Fn-Fin)
;; C-Home         Ir al principio del documento (Mac: C-Fn-Home)
;; C-Fin          Ir al final del documento (Mac: C-Fn-Fin)
;;
;; ?? (global-set-key [M-f4]   'delete-frame)
;;
;;
;;

;; Opcion genérica
(setq frame-title-format "%b - GNU Emacs") ; Make the frame a bit more useful.

;; Look and feel
(set-foreground-color "gray")
(set-background-color "black")
(set-cursor-color "red")
(set-mouse-color "green")
(set-border-color "light green")

;; Mostrar la hora con este formato
(setq display-time-string-forms
      '(24-hours ":" minutes))
(display-time)

;; Activar números de columnas
(column-number-mode)

; At least this mode won't do anything stupid.
(setq default-major-mode 'text-mode)

(setq pop-up-windows nil                ; Don't change my windowconfiguration.
      european-calendar-style t         ; Use european date format.
      pop-up-windows nil                ; Assure my window-configuration is kept.
      require-final-newline t)          ; Fix rotten daimi-setup.

;; Go spanish:
(set-keyboard-coding-system 'iso-latin-1)
(set-terminal-coding-system 'iso-latin-1)

;; Ignorar 'case' en búsquedas
(setq case-fold-search t)

;(if window-system
;
;    ;; Place scroll-bar
; ;;   (set-scroll-bar-mode 'right)
;
;    ;; hide toolbar
; ;;   (tool-bar-mode nil)
;)
(setq scroll-step 1)

;; Nada de pitidos
(setq ring-bell-function 'ignore)

;; Ya vale de mensajes al arrancar
(setq inhibit-startup-message t)

;; Mostrar paréntesis
(show-paren-mode t)

;; Paso de menus
(menu-bar-mode nil)

;; Reabrir los buffers antiguos
(desktop-load-default)
(desktop-read)

;; Auto comprimir..
(auto-compression-mode t)

;; Usar SHIFT y los cursores para marcar regiones
(require 's-region)

;; Usar UTF-8 en todos lados...
(set-language-environment 'utf-8)
(set-default-coding-systems 'utf-8)
(set-selection-coding-system 'utf-8)
(set-locale-environment "es.UTF-8")
(prefer-coding-system 'utf-8)
(setq utf-translate-cjk-mode nil) ; disable CJK coding/encoding

;; Recordar cosillas...
(custom-set-variables
 '(global-font-lock-mode t nil (font-lock))
 '(save-place t nil (saveplace))
 '(transient-mark-mode t))

;; Colorines
(custom-set-faces
 '(font-lock-comment-face ((t (:foreground "forest green"))))
 '(highlight ((t (:foreground "blue" :background "indian red"))))
 '(isearch ((t (:inherit region))))
 '(modeline ((t (:foreground "black" :background "steelblue"))))
 '(region ((t (:foreground "white" :background "midnight blue"))))
 '(show-paren-match-face ((t (:foreground "black"
                                          :background "steel blue")))))

;; Teclas
;; Si usas o has usado windows estas teclas te ayudarán a no
;; volverte loco.
(global-set-key [end]    'end-of-line)
(global-set-key [home]   'beginning-of-line)
(global-set-key [delete] 'delete-char)
(global-set-key [C-home] 'beginning-of-buffer)
(global-set-key [C-end]  'end-of-buffer)
(global-set-key [?\C-z]  'undo)
(global-set-key [M-f4]   'delete-frame)

;; Keybindings -- Global
;; Ctrl+f para search and replace (Ctrl+S == search)
(global-set-key [(ctrl f)] 'query-replace)

;; Keybindings -- MacOS X
(cond
 ((eq system-type 'Darwin)
  ;;Set option to be the Meta key
  ;;Set apple+c/x/v to copy/cut/paste
  ;;Set apple+q to close emacs (save buffers first)
  ;;Set apple+w to delete frame
  ;;Set apple+e to kill current buffer
  ;;Set apple+z to undo
  ;;Set apple+s to save buffer
  ;;Set apple+n to make frame
  (setq mac-command-key-is-meta nil)
  (global-set-key [(alt v)] 'yank)
  (global-set-key [(alt c)] 'kill-ring-save)
  (global-set-key [(alt x)] 'kill-region)
  (global-set-key [(alt q)] 'save-buffers-kill-emacs)
  (global-set-key [(alt w)] 'delete-frame)
  (global-set-key [(alt e)] 'myKillBuffer)
  (global-set-key [(alt z)] 'undo)
  (global-set-key [(alt s)] 'save-buffer)
  (global-set-key [(alt n)] 'make-frame)
))

;; Backups
;; Centralizar backups, en vez de tener todo el disco lleno
;; de fichero que terminan con (~) por todas partes.
(setq make-backup-files t)
(setq auto-save-default nil)
(setq delete-old-versions t)

(defun make-backup-file-name (file)
  "Create the non-numeric backup file name for FILE."
  (require 'dired)
  (let (path)
    (cond ((eq system-type 'windows-nt)
           (setq file (dired-replace-in-string ":" "" file))
           (setq path "D:/backups/"))
          (t
	   ;; Directorio
           (setq path "~/.backups/")
	   (unless (file-exists-p path)
	     (make-directory path t))
	   ))
    (concat path
            (dired-replace-in-string "/" "~" file)
            (format-time-string "~%Y%m%d%H%M%S~")
            (car (last (split-string file "/"))))))

;; Desktop
;; Recuerda cosas...
(when (fboundp 'desktop-load-default)
  (desktop-load-default)
  (mapcar
   (lambda (sym) (add-to-list 'desktop-globals-to-save sym))
   '((buffer-name-history      . 20)
     (dired-regexp-history     . 20)
     (extended-command-history . 30)
     (file-name-history        . 100)
     (grep-history             . 30)
     (minibuffer-history       . 50)
     (query-replace-history    . 60)
     (read-expression-history  . 60)
     (regexp-history           . 60)
     (regexp-search-ring       . 20)
     (search-ring              . 20)
     (shell-command-history    . 50)))
  (desktop-read))
</pre>

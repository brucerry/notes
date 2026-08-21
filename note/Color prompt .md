## Windows

Open profile in notepad
```powershell
notepad $PROFILE
```

Add custom `prompt` function
```powershell
function prompt {
    Write-Host "PS " -NoNewline -ForegroundColor Cyan
    Write-Host "$(Get-Location)> " -NoNewline -ForegroundColor Yellow
    return " "
}
```

Save the file and restart terminal

*If scripts are blocked, run PowerShell once as normal user:*
```
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

## Linux

Open ~/.bashrc with editor

Enable `force_color_prompt`
```bash
force_color_prompt=yes
```

Save the new setting as follow
```bash
## Prompt colors (ANSI + 256-color examples)
## Basic bright colors
RED="1;31"
GREEN="1;32"
YELLOW="1;33"
BLUE="1;34"
MAGENTA="1;35"
CYAN="1;36"
WHITE="1;37"

## 256-color accents (works best in *-256color terminals)
ORANGE="1;38;5;208"
GOLD="1;38;5;220"
PINK="1;38;5;213"
VIOLET="1;38;5;141"
TEAL="1;38;5;44"
SKY="1;38;5;117"
MINT="1;38;5;49"
LIME="1;38;5;118"
SALMON="1;38;5;209"
CORAL="1;38;5;203"

## Ubuntu release inspired prompt themes.
PROMPT_THEME="${PROMPT_THEME:-noble}"
prompt_theme() {
    case "$1" in
        list|--list|-l)
            echo "Available prompt themes:"
            echo "  default"
            echo "  xenial   (16.04 LTS)"
            echo "  bionic   (18.04 LTS)"
            echo "  focal    (20.04 LTS)"
            echo "  jammy    (22.04 LTS)"
            echo "  noble    (24.04 LTS)"
            echo "  oracular (24.10)"
            echo "  plucky   (25.04)"
            echo "  resolute (26.04 LTS)"
            return 0
            ;;
        default|"")
            PROMPT_USER_COLOR="$RED"
            PROMPT_AT_HOST_COLOR="$GREEN"
            PROMPT_PATH_COLOR="01;34"
            ;;
        xenial|16.04)
            PROMPT_USER_COLOR="1;38;5;131"
            PROMPT_AT_HOST_COLOR="1;38;5;208"
            PROMPT_PATH_COLOR="01;34"
            ;;
        bionic|18.04)
            PROMPT_USER_COLOR="1;38;5;91"
            PROMPT_AT_HOST_COLOR="1;38;5;214"
            PROMPT_PATH_COLOR="01;36"
            ;;
        focal|20.04)
            PROMPT_USER_COLOR="1;38;5;166"
            PROMPT_AT_HOST_COLOR="1;38;5;173"
            PROMPT_PATH_COLOR="01;37"
            ;;
        jammy|22.04)
            PROMPT_USER_COLOR="1;38;5;135"
            PROMPT_AT_HOST_COLOR="1;38;5;205"
            PROMPT_PATH_COLOR="01;117"
            ;;
        noble|24.04)
            PROMPT_USER_COLOR="1;38;5;208"
            PROMPT_AT_HOST_COLOR="1;38;5;39"
            PROMPT_PATH_COLOR="01;45"
            ;;
        oracular|24.10)
            PROMPT_USER_COLOR="1;38;5;202"
            PROMPT_AT_HOST_COLOR="1;38;5;226"
            PROMPT_PATH_COLOR="01;33"
            ;;
        plucky|25.04)
            PROMPT_USER_COLOR="1;38;5;214"
            PROMPT_AT_HOST_COLOR="1;38;5;51"
            PROMPT_PATH_COLOR="01;44"
            ;;
        resolute|26.04)
            PROMPT_USER_COLOR="1;38;5;89"
            PROMPT_AT_HOST_COLOR="1;38;5;214"
            PROMPT_PATH_COLOR="01;38;5;67"
            ;;
        *)
            echo "Unknown theme: $1"
            echo "Use: prompt_theme list"
            return 1
            ;;
    esac
}

prompt_theme list
if [ -n "${PROMPT_THEME:-}" ]; then
    prompt_theme "$PROMPT_THEME"
fi

if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[0${PROMPT_USER_COLOR}m\]\u\[\033[0${PROMPT_AT_HOST_COLOR}m\]@\h\[\033[00m\]:\[\033[${PROMPT_PATH_COLOR}m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
```

Relaunch terminal or apply setting at runtime
```bash
source ~/.bashrc
```

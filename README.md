# SETUP_LINUX
Set up linux by Ronny.

## ✨ Zsh Prompt
```zsh
PROMPT='%F{cyan}%~%f '
alias ls='ls --color=auto'

# ปิดสีไฮไลต์พื้นหลังเขียวของโฟลเดอร์ใน Windows
export LS_COLORS=$LS_COLORS:'ow=01;34:'

# เปิดใช้งานการแปลงตัวแปรใน PROMPT
setopt PROMPT_SUBST

# ล้างค่า ~d เดิม
unhash d 2>/dev/null

PROMPT='%F{cyan}${PWD/#\/mnt\/d/~}%f '

cd /mnt/d

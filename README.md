symfony2-bash-complete
======================

Bash completion for app/console

copy and paste into ~/.profile

```bash
alias sfc="php app/console"

_console()
{
    local cur prev cmd possible
    COMPREPLY=()
    cur=${COMP_WORDS[COMP_CWORD]}
    prev="${COMP_WORDS[COMP_CWORD-1]}"
    case "$prev" in
      sfc)
        possible=`sfc --list --no-ansi | sed '1!G;h;$!d' | sed -n '/Available commands:/q;p' | sed '1!G;h;$!d' | grep '^  ' | cut -d\  -f3 | grep "^$cur"`;
        ;;
      *)
        if [[ ${COMP_CWORD} == 2 ]] ; then
          possible=`sfc $prev --help --no-ansi | sed -n '/Help:/q;p' | sed '1!G;h;$!d' | sed -n '/Options:/q;p'  | sed '1!G;h;$!d' | grep '^ ' | cut -d\  -f2 | grep "^$cur"`;
        fi
        ;;
    esac
    COMPREPLY=( $(compgen -W "${possible}" -- ${cur}) )
    return 0
}

complete -F _console sfc
COMP_WORDBREAKS=${COMP_WORDBREAKS//:}
```

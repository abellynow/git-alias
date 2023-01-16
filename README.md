## git-alias

```
	st = status
	c = commit
	fixup = "!f() { git commit --fixup=$1; }; f"
	co = checkout
	col = checkout @{-1}
	unadd = reset HEAD
	ua = reset HEAD
	discard = checkout -p --
	dis = checkout -p --
	uncommit = reset --soft HEAD^
	uc = reset --soft HEAD^
	slog = "!f(){ git log subrepo/$@/fetch; }; f"
	amend = commit --amend -C HEAD
	diff-file-last-commit = "!f() { \
            project_root_dir=$(git rev-parse --show-toplevel); \
            echo finding full file path of $1 in $project_root_dir; \
            filepath=$(find $project_root_dir -type f -name $1); \
            echo full file path $filepath; \
            last_modified_commit_hash=$(git rev-list -1 HEAD $filepath); \
            echo last commit file modified $last_modified_commit_hash; \
            git diff $last_modified_commit_hash^ $filepath; \
	}; f"
	blm = "!f() { \
		if [ \"$1\" != \"\" ]; then \
			git for-each-ref --sort='-authordate' --format='%(authordate)%09%(objectname:short)%09%(refname)' refs/heads | sed -e 's-refs/heads/--' | head $1; \
		else \
			git for-each-ref --sort='-authordate' --format='%(authordate)%09%(objectname:short)%09%(refname)' refs/heads | sed -e 's-refs/heads/--'; \
		fi; \
	}; f"
	g = grep --break --heading --line-number
	puff = pull --ff-only
	ff = merge --ff-only
	pushup = "!git push --set-upstream upstream \"$(git rev-parse --abbrev-ref HEAD)\""
	prom = pull --rebase origin master
	newin = log --diff-filter=A --
	findin = "!f() { \
	    branches=$(git branch --no-merged); \
	    for b in $branches; do \
		dt=$(git log --diff-filter=A "$b" -- "$1"); \
		if [ \"$dt\" != \"\" ]; then \
		    echo Found in '\"'$b'\"':; \
		    git log --diff-filter=A "$b" -- "$1"; \
		fi; \
	    done; \
	}; f"
	squash = rebase -i --autosquash
```
## Bash prompt (.bashrc)

```
parse_git_branch() {
     git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}

export PS1="\[\033[01;32m\]\u@\h\[\033[00m\]\[\033[01;35m\][\A]\[\033[00m\]:\[\033[01;34m\]\w\[\033[31m\]\$(parse_git_branch)\[\033[00m\]$ "
```

## Vim script (.vimrc)

```
function! s:swap_lines(n1, n2)
    let line1 = getline(a:n1)
    let line2 = getline(a:n2)
    call setline(a:n1, line2)
    call setline(a:n2, line1)
endfunction

function! s:swap_up()
    let n = line('.')
    if n == 1
        return
    endif

    call s:swap_lines(n, n - 1)
    exec n - 1
endfunction

function! s:swap_down()
    let n = line('.')
    if n == line('$')
        return
    endif

    call s:swap_lines(n, n + 1)
    exec n + 1
endfunction

noremap <silent> <a-up> :call <SID>swap_up()<CR>
noremap <silent> <a-down> :call <SID>swap_down()<CR>
```

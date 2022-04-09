# brew services helper

```bash
_listRunServices=()

# List only running services
bslist () {
  brew services --JSON | jq -c '.[] | select( .status == "started" ) | .name'
}

# The list of currently running services is stored in the terminal session and stop them
bsstop () {
  _list=(`brew services --JSON | jq -c '.[] | select( .status == "started" ) | .name' | tr -d '"'`)

  for _name in "${_list[@]}"
  do
    _listRunServices+=($_name)
    brew services -qv stop $_name
  done
}

# Run a list of saved services in terminal session
bsrun () {
  for _name in "${_listRunServices[@]}"
  do
    brew services -qv start $_name;
  done

  _listRunServices=();
}
```

### TODO
Replace `_listRunServices` on storage in `.oh-my-zsh/brew-services.json`

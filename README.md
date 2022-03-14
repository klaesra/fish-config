# fish-config

# Auto run `nvm use` in folders with .nvmrc needs nvm plugin or other executable
function __check_nvm --on-variable PWD --description 'Do nvm stuff'
    if test -f .nvmrc
        set node_version (nvm current)
        if test -z "$node_version"
            # Set version to None to ensure the string match will fail if node_version wasn't set
            set node_version None
        end

        set nvmrc_node_version (nvm list | grep (cat .nvmrc))

        if set -q $nvmrc_node_version
            nvm install
        else if string match -q -- "*$node_version*" $nvmrc_node_version
            # already current node version
        else
            nvm use
        end
    end
end

__check_nvm

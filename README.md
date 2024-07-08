# debug-terraform-provider

export GOPATH

```
export GOPATH=~/go
```

clone repo to $GOPATH/src/github.com/hashicorp

```
cd $GOPATH/src/github.com/hashicorp
git clone git@github.com:nickdala/terraform-provider-azuread.git  
```

Build the provider

```
cd /Users/nick/go/src/github.com/hashicorp/terraform-provider-azuread
make tools
make build
make test
```

Build for debugging

```
go build -gcflags="all=-N -l" -trimpath -o terraform-provider-azuread
```


create `.terraformrc` in home directory

```terraform
provider_installation {
  dev_overrides {
    "hashicorp/azuread" = "/Users/nick/go/src/github.com/hashicorp/terraform-provider-azuread"
  }

  direct {}
}
```

VS CODE `launch.json` file in .`vscode` directory

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        
        {
            "name": "Debug Terraform Provider",
            "type": "go",
            "request": "launch",
            "mode": "debug",
            // this assumes your workspace is the root of the repo
            "program": "${workspaceFolder}",
            "env": {},
            "args": [
                "-debug",
            ]
        }
    ]
}
```

Copy the `TF_REATTACH_PROVIDERS` and export it in the Terraform project

```
export TF_REATTACH_PROVIDERS='{"registry.terraform.io/hashicorp/azuread":{"Protocol":"grpc","ProtocolVersion":5,"Pid":45668,"Test":true,"Addr":{"Network":"unix","String":"/var/folders/yj/d_8vxxn95fxc94y0wj_2zrxr0000gn/T/plugin1679099462"}}}'
```

Set log path

```
export TF_LOG_PATH=/Users/nick/tmp/app-registration/terraform.log
```

Set the log level

```
export TF_LOG=DEBUG
```

Run `terraform apply`

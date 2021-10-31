# orka-actions-up

Run self-hosted, macOS workflows on MacStadium's Orka. 


## Example workflow

```
on:
  push:
    branches:
      - main

jobs:
  job1:
    runs-on: ubuntu-latest
    steps:
    - name: Job 1
      id: job1
      uses: jeff-vincent/orka-actions-up@main
      with:
        orkaIP: http://10.221.188.100
        orkaUser: ${{ secrets.ORKA_USER }}
        orkaPass: ${{ secrets.ORKA_PASS }}
        orkaBaseImage: gha_bigsur_v3.img
        githubUser: ${{ secrets.GH_USER }}
        githubPat: ${{ secrets.GH_PAT }}
        githubRepoName: orka-actions-up
        vpnUser: ${{ secrets.VPN_USER }}
        vpnPassword: ${{ secrets.VPN_PASSWORD }}
        vpnAddress: ${{ secrets.VPN_ADDRESS }}
        vpnServerCert: ${{ secrets.VPN_SERVER_CERT }}
    outputs:
      vm-name: ${{ steps.job1.outputs.vm-name }}
         
  job2:
    needs: job1
    runs-on: [self-hosted, "${{ needs.job1.outputs.vm-name }}"]
    steps:
    - name: Job 2
      id: job2
      run: |
        sw_vers
  job3:
    if: always()
    needs: [job1, job2]
    runs-on: ubuntu-latest
    steps:
    - name: Job 3
      id: job3
      uses: jeff-vincent/orka-actions-down@main
      with:
        orkaIP: http://10.221.188.100
        orkaUser: ${{ secrets.ORKA_USER }}
        orkaPass: ${{ secrets.ORKA_PASS }}
        orkaBaseImage: gha_bigsur_v3.img
        githubUser: ${{ secrets.GH_USER }}
        githubPat: ${{ secrets.GH_PAT }}
        githubRepoName: orka-actions-up
        vpnUser: ${{ secrets.VPN_USER }}
        vpnPassword: ${{ secrets.VPN_PASSWORD }}
        vpnAddress: ${{ secrets.VPN_ADDRESS }}
        vpnServerCert: ${{ secrets.VPN_SERVER_CERT }}
        vmName: ${{ needs.job1.outputs.vm-name }}
```

## job2 example output

```
Run sw_vers
  sw_vers
  shell: /bin/bash -e {0}
ProductName:	macOS
ProductVersion:	11.3
BuildVersion:	20E232
```

# Ceph-Benchmarking-tool
A wrapper tool that uses ansible , FIO and IOZONE to perform benchmarking excercise on your storage.

# Prerequisite
```
yum install -y python-pip git python-devel 
pip install --upgrade pip 
pip install ansible 
pip install python-novaclient 
pip install python-clinderclient 
pip install shade
```
# How to use this repo
Currently this tool uses openstack to provision instances / volumes and prepare them for benchmarking.
- To setup client nodes
   - Edit ```ansible_playbook/group_vars/all.yml``` with your preffered values
   - Run ``` cd ansible_playbook ; ansible-playbook launch_openstack_resources.yml ```
   - Don't forgot to update ``/etc/hosts`` and ``ansible_playbook/inventory/hosts`` ( sorry its manual at this time )
   
- To start Benchmarking
   - Edit ``fio/run_me.sh`` or ``iozone/*k_tests.csv`` based on the the tool you have selected for benchmarking
   - Run ``cd ansible_playbook; ansible-playbook start_benchmarking.yml -i inventory/hosts `` to start benchmarking
   - Benchmarking will run as root under a screen session, so use ``screen -ls ; screen -r`` to monitor your test
   
- To Gather results
   - Run `` ansible-playbook -i inventory/hosts gather_results.yml --extra-vars "run=NameYourRun" ``
   - Output will be collected in ``ansible_playbook/results/results_$Bench_tool_name `` directory
   
- Merge multiple output files to single file and grab the ``*.summary`` files and put them into spreadsheet 
```
cd ansible_playbook/results/results_$Bench_tool_name
for i in read write readwrite randread randwrite ; do cat *_$i.summary >> $i.summary ; done 
for i in read write randread randwrite; do cat $i.summary >> final_result.summary; done 
cp readwrite.summary final_result_readwrite.summary
```
# Contribute to make it Cool !!!
I know there are several improvement points. So let's colloborate and contribute to make this tool robust and useful for us / others.

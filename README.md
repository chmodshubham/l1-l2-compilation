# O-DU

Move `G_Rel_l2.tar.gz` into server 4

cd /opt/cek/intel-flexran/bin/nr5g/gnb/l1/
./l1.sh -xran
cd ../../../../
source set_env_var.sh -d
cd bin/nr5g/gnb/l1/
./l1.sh -xran

![image](https://github.com/ShubhamKumar89/o-du/assets/97805339/0486c244-2e9e-43a6-a3f4-81d84f515a5f)

![image](https://github.com/ShubhamKumar89/o-du/assets/97805339/635f52d8-439b-4e40-a5cb-faa5c8351f51)

now u will be moved to the application console of PHY

![image](https://github.com/ShubhamKumar89/o-du/assets/97805339/dbd64875-e0f0-403a-8a32-313cf7eabde9)

exit the console 

cd ../../../../
cd wls_mod/

cd /home/four/
tar -xzf G_Rel_l2.tar.gz

cd l2/build/odu/
vim makefile

https://hackmd.io/@ChiehChun/HyNXWdATI/https%3A%2F%2Fhackmd.io%2F%40Tony-Chen%2FSJEVopO4O

```patch
# Add to the linker options the platform specific components
L_OPTS+=-lnsl -lrt -lm -lpthread -lsctp
ifeq ($(PHY), INTEL_L1)
-       L_OPTS+=-L/home/oran/F_l1_G_l2/phy/wls_lib -lwls                         \
+       L_OPTS+=-L/opt/cek/intel-flexran/wls_mod -lwls                           \
-       -lhugetlbfs -lnuma -ldl -L/home/oran/DPDK/dpdk-stable-20.11.3/build/lib/                        \
+       -lhugetlbfs -lnuma -ldl -L/opt/cek/dpdk-21.11/build/lib/                        \
        -lrte_gso -lrte_acl -lrte_hash -lrte_bbdev -lrte_ip_frag -lrte_bitratestats -lrte_ipsec        \
        -lrte_bpf -lrte_jobstats -lrte_telemetry -lrte_kni -lrte_kvargs -lrte_latencystats -lrte_port  \
        -lrte_lpm -lrte_power -lrte_mbuf -lrte_rawdev -lrte_member -lrte_cfgfile -lrte_mempool         \
                  -lrte_cmdline -lrte_rcu -lrte_compressdev -lrte_reorder -lrte_cryptodev -lrte_rib              \
                  -lrte_distributor -lrte_meter  -lrte_ring -lrte_eal -lrte_metrics -lrte_sched -lrte_efd        \
                  -lrte_net -lrte_security -lrte_ethdev -lrte_pci -lrte_stack -lrte_eventdev -lrte_pdump         \
                  -lrte_table -lrte_fib -lrte_pipeline -lrte_timer -lrte_flow_classify -lrte_vhost               \
        -lrte_gro
endif
```

cd ../../src/
sudo rm -r dpdk_lib/ wls_lib/
mkdir dpdk_lib/
mkdir wls_lib/

cd /opt/cek/intel-flexran/wls_mod/
cp -r * /home/four/l2/src/wls_lib/

cd /opt/cek/dpdk-21.11/lib/eal/include/

cp rte_branch_prediction.h rte_common.h rte_dev.h rte_log.h rte_pci_dev_feature_defs.h rte_bus.h rte_compat.h rte_debug.h rte_eal.h rte_per_lcore.h /home/four/l2/src/dpdk_lib/

cd /opt/cek/dpdk-21.11/config/

cp rte_config.h /home/four/l2/src/dpdk_lib/

cd /opt/cek/dpdk-21.11/lib/eal/linux/include/
cp rte_os.h /home/four/l2/src/dpdk_lib/

cd /home/four/l2/build/src/

make help

https://hackmd.io/DsgRSkR7RpSsdwmhdQEVNQ?view#Step-4-Compile-O-DU-High-with-INTEL_L1-option        
make clean_odu

make odu PHY=INTEL_L1 MACHINE=BIT64 MODE=TDD 
make clean_odu


vim /home/four/l2/src/mt/mt_ss.c

```patch
- hdl = WLS_Open(WLS_DEVICE_NAME, WLS_MASTER_CLIENT, &nWlsMacMemorySize, &nWlsPhyMemorySize);
+ void* WLS_Open(const char *ifacename, unsigned int mode, uint64_t *nWlsMacMemorySize, uint64_t *nWlsPhyMemorySize, uint32_t nWlsULEnqueueSize);
```

make odu PHY=INTEL_L1 MACHINE=BIT64 MODE=TDD 
make clean_odu

cd ../../../

mkdir install_g++-4.8
cd install_g++-4.8/
sudo apt update
wget http://mirrors.kernel.org/ubuntu/pool/universe/g/gcc-4.8/g++-4.8_4.8.5-4ubuntu8_amd64.deb 
wget http://mirrors.kernel.org/ubuntu/pool/universe/g/gcc-4.8/libstdc++-4.8-dev_4.8.5-4ubuntu8_amd64.deb 
wget http://mirrors.kernel.org/ubuntu/pool/universe/g/gcc-4.8/gcc-4.8-base_4.8.5-4ubuntu8_amd64.deb 
wget http://mirrors.kernel.org/ubuntu/pool/universe/g/gcc-4.8/gcc-4.8_4.8.5-4ubuntu8_amd64.deb 
wget http://mirrors.kernel.org/ubuntu/pool/universe/g/gcc-4.8/libgcc-4.8-dev_4.8.5-4ubuntu8_amd64.deb 
wget http://mirrors.kernel.org/ubuntu/pool/universe/g/gcc-4.8/cpp-4.8_4.8.5-4ubuntu8_amd64.deb 
wget http://mirrors.kernel.org/ubuntu/pool/universe/g/gcc-4.8/libasan0_4.8.5-4ubuntu8_amd64.deb  
sudo apt install ./gcc-4.8_4.8.5-4ubuntu8_amd64.deb ./gcc-4.8-base_4.8.5-4ubuntu8_amd64.deb ./libstdc++-4.8-dev_4.8.5-4ubuntu8_amd64.deb ./cpp-4.8_4.8.5-4ubuntu8_amd64.deb ./libgcc-4.8-dev_4.8.5-4ubuntu8_amd64.deb ./libasan0_4.8.5-4ubuntu8_amd64.deb ./g++-4.8_4.8.5-4ubuntu8_amd64.deb


g++-4.8 --version

![image](https://github.com/ShubhamKumar89/o-du/assets/97805339/5d84e9c6-6ef5-48a6-9840-9285659f5a84)

gcc-4.8 --version

![image](https://github.com/ShubhamKumar89/o-du/assets/97805339/498b906e-94b0-493f-a708-965033e897ec)

mv /usr/bin/gcc-4.8 /usr/bin/gcc

cd l2/build/odu/

make clean_odu
make odu PHY=INTEL_L1 MACHINE=BIT64 MODE=TDD 

cd /opt/cek/intel-flexran/bin/nr5g/gnb/l1/
./l1.sh -xran
cd ../../../../
source set_env_var.sh -d

cd /opt/cek/intel-flexran/bin/nr5g/gnb/l1/
./l1.sh -xran

open new terminal

cd /opt/cek/intel-flexran/
source set_env_var.sh -d

cd /home/four/l2/bin/cu_stub/
sudo ./cu_stub




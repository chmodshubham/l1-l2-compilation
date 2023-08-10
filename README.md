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

make clean_odu





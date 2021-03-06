
La serie Ls está optimizada para las cargas de trabajo que requieren almacenamiento temporal de baja latencia, como bases de datos NoSQL, incluyendo Cassandra, MongoDB, Cloudera y Redis. La serie LS ofrece hasta 32 vCPU, con el [procesador Intel® Xeon® de la familia E5 v3](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). La serie LS obtiene el mismo rendimiento de CPU que las series G/GS y viene con 8 GiB de memoria por vCPU.  

## <a name="ls-series"></a>Serie Ls

ACU: 180-240
 
| Tamaño          | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento de almacenamiento temporal en caché y máx.: E/S por segundo / MBps (tamaño de caché en GiB) | Rendimiento de disco no en caché máx.: E/S por segundo / Mbps | Nº máx. NIC / rendimiento de red esperado (Mbps) | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standard_L4s  | 4    | 32   | 678   | 8              | ND / ND (0)          | 5000 / 125                               | 2 / 4000       | 
| Standard_L8s  | 8    | 64   | 1,388 | 16             | ND / ND (0)          | 10 000 / 250                              | 4 / 8000  | 
| Standard_L16s | 16   | 128  | 2,807 | 32             | ND / ND (0)          | 20 000 / 500                              | 8 / 6000 - 16000 &#8224; | 
| Standard_L32s* | 32 | 256  | 5,630 | 64             | ND / ND (0)          | 40 000 / 1000                            | 8 / 20000 | 
 

El rendimiento máximo de disco que es posible con una máquina virtual de la serie Ls puede estar limitado por el número, el tamaño y la fragmentación de cualquier disco asociado. Para obtener más información, consulte [Premium Storage: almacenamiento de alto rendimiento para cargas de trabajo de máquinas virtuales de Azure](../articles/storage/common/storage-premium-storage.md). 

*La instancia está aislada en el hardware dedicado a un solo cliente.


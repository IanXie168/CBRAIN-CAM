# Stephan's project notebook

### October 18

Trying to get main.py to run. Problems
- no variable LAT in mean and std files --> Create myself in data_exploration.ipynb

```
<Closed HDF5 file>
Exception in thread Thread-7:
Traceback (most recent call last):
  File "/home/s/S.Rasp/repositories/CBRAIN/dataLoad.py", line 155, in accessData
    arr = fileReader[k][:,s:s+l].T
  File "h5py/_objects.pyx", line 54, in h5py._objects.with_phil.wrapper (/home/ilan/minonda/conda-bld/h5py_1496889914775/work/h5py/_objects.c:2846)
  File "h5py/_objects.pyx", line 55, in h5py._objects.with_phil.wrapper (/home/ilan/minonda/conda-bld/h5py_1496889914775/work/h5py/_objects.c:2804)
  File "/home/s/S.Rasp/.conda/envs/cbrain/lib/python3.6/site-packages/h5py/_hl/group.py", line 169, in __getitem__
    oid = h5o.open(self.id, self._e(name), lapl=self._lapl)
  File "h5py/_objects.pyx", line 54, in h5py._objects.with_phil.wrapper (/home/ilan/minonda/conda-bld/h5py_1496889914775/work/h5py/_objects.c:2846)
  File "h5py/_objects.pyx", line 55, in h5py._objects.with_phil.wrapper (/home/ilan/minonda/conda-bld/h5py_1496889914775/work/h5py/_objects.c:2804)
  File "h5py/h5o.pyx", line 190, in h5py.h5o.open (/home/ilan/minonda/conda-bld/h5py_1496889914775/work/h5py/h5o.c:3740)
ValueError: Not a location (Invalid object id)
```
Happens at the end of the epochs, training works probably still. Doesn't appear for convolution with 50 epochs

### October 22

Running on GPU, which conda environment to use? Copy cbrain to cbrain_gpu, then conda install tensorflow-gpu.

This is Pierre's latest:

mpiexec python  ./main.py --run_validation=true --randomize=true --batch_size=4096 --optim=adam --lr=1e-3 --frac_train=0.8 --log_step=100 --epoch=50 --randomize=true --input_names='TAP,QAP,OMEGA,SHFLX,LHFLX,LAT,dTdt_adiabatic,dQdt_adiabatic' --hidden=32,32 --convo=false # > atoutfile

Let's try running (with sample directory):

python  ./main.py --run_validation=true --randomize=true --batch_size=4096 --optim=adam --lr=1e-3 --frac_train=0.8 --log_step=10 --epoch=10 --randomize=true --input_names='TAP,QAP,OMEGA,SHFLX,LHFLX,LAT,dTdt_adiabatic,dQdt_adiabatic' --hidden=32,32 --convo=false --addon sample

Ok seems to work, even though I am getting the HDF5 error from above, which doesn't seem to matter.

Now let's try Tensorboard: tensorboard --logdir logs/ --> Running at http://localhost:6005/

Now let's run the full dataset with 50 epochs:

python ./main.py --run_validation=true --randomize=true --batch_size=4096 --optim=adam --lr=1e-3 --frac_train=0.8 --log_step=100 --epoch=50 --randomize=true --input_names='TAP,QAP,OMEGA,SHFLX,LHFLX,LAT,dTdt_adiabatic,dQdt_adiabatic' --hidden=32,32 --convo=false

Wait the LAT std is missing. let's fix that! Again in jupyter notebook


Open question:
- How does it do train/valid split, could there be a similar problem I am having with the postprocessing?
- Why is there a data/ directory in my repo?
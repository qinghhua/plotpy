Making executable Windows programs
==================================

Applications developed with Python may be deployed using specialized tools 
like `py2exe` or `cx_Freeze`. These tools work as extensions to Python builtin 
`distutils` module and converts Python scripts into executable Windows 
programs which may be executed without requiring a Python installation.

Making such an executable program may be a non trivial task when the script 
dependencies include libraries with data or extensions, such as `PyQt4` or 
`plotpy`. This task has been considerably simplified thanks to 
the helper functions provided by :py:mod:`plotpy.disthelpers`.

Example
~~~~~~~

This example is included in `plotpy` source package (see the 
``deployment_example`` folder at source package root directory).

Simple example script named ``simpledialog.pyw`` which is based on `plotpy`::

    from plotpy.plot import ImageDialog
    from plotpy.builder import make
    
    class VerySimpleDialog(ImageDialog):
        def set_data(self, data):
            plot = self.get_plot()
            item = make.trimage(data)
            plot.add_item(item, z=0)
            plot.set_active_item(item)
            plot.replot()
    
    if __name__ == "__main__":
        import numpy as np
        from plotpy import qapplication
        qapplication()
        dlg = VerySimpleDialog()
        dlg.set_data(np.random.rand(100, 100))
        dlg.exec_()

The ``create_exe.py`` script may be written as the following::

    from plotpy import disthelpers as dh
    dist = dh.Distribution()
    dist.setup('example', '1.0', 'plotpy app example', 'simpledialog.pyw')
    dist.add_modules('plotpy')
    dist.build_cx_freeze()  # use `build_py2exe` to use py2exe instead

Make the Windows executable program by simply running the script::

    python create_exe.py

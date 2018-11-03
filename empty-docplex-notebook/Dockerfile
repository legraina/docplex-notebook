FROM jupyter/scipy-notebook

# add cplex and cpoptimizer to the path and then docplex
ENV PATH="/home/jovyan/CPLEX_Studio/cplex/bin/x86-64_linux:/home/jovyan/CPLEX_Studio/cpoptimizer/bin/x86-64_linux:$PATH"
RUN pip install docplex

# copy exemples
COPY ./docplex-examples /home/jovyan/work/examples

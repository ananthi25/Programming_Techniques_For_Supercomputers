#include <stdio.h>
#include <stdlib.h>
#include <mpi.h>
#define ms 2

int main(int argc,char* argv[])
{
    int i,j,k;
    int x,c;
    int matrix_a[ms][ms];
    int matrix_b[ms][ms];
    int matrix_c[ms][ms];
    int myrank, p;
    int NRPE;
    double starttime, endtime;

    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &myrank);
    MPI_Comm_size(MPI_COMM_WORLD, &p);
    MPI_Status status;

    NRPE = ms / p;

    if(myrank == 0)
    {
         printf("\nTCE 3411 Parellel Processing : Matrix Multiplication\n");
         printf("\n Ananthi 1805009\n");

         printf("\nDate : %s",__DATE__);
         printf("\nTime : %s",__TIME__);

        //matrix a
        printf("\n   matrix a \n");
        printf("--------------\n");

        for(i=0; i<ms; ++i)
            for(j=0; j<ms; ++j)
                matrix_a[i][j] = rand() % 10;

        for(i=0; i<ms; ++i)
        {
            for(j=0; j<ms; ++j)
                printf("%3d", matrix_a[i][j]);
            printf("\n");
        }
        //matrix b
        printf("\n   matrix b \n");
        printf("--------------\n");

        for(x=0; x<ms; ++x)
            for(c=0; c<ms; ++c)
                matrix_b[x][c] = rand() % 10;

        for(x=0; x<ms; ++x)
        {
            for(c=0; c<ms; ++c)
                printf("%3d", matrix_b[x][c]);
            printf("\n");
        }

    }

    //Broadcast Matrix B values to all proccess
    for(i=0; i < ms; i++)
    {
        MPI_Bcast(matrix_b[i], ms*ms, MPI_INT, 0, MPI_COMM_WORLD);
    }
    printf("\n MATRIX B by Process: %d\n", myrank);
    for(x=0; x<ms; ++x)
    {
        for(c=0; c<ms; ++c)
            printf("%3d", matrix_b[x][c]);
        printf("\n");
    }
    //End of Broadcast

    //Sending of each row of Matrix A to all process
    for(i=0; i<p; i++)
    {
      for(j=0; j<ms; j++)
       {
            MPI_Send(&matrix_a[j], ms*NRPE, MPI_INT, i, 0, MPI_COMM_WORLD);
            NRPE++;
       }
    }
    //end of sending

    //Receiving of each row of Matrix A per process
    MPI_Recv(matrix_a, ms*NRPE, MPI_INT, 0, 0, MPI_COMM_WORLD, &status);


    //Multiplication Area
    starttime = MPI_Wtime();
    for (k=0; k<ms; k++)
    for (i=0; i<ms; i++) {
      matrix_c[i][k] = 0;
      for (j=0; j<ms; j++)
        matrix_c[i][k] = matrix_c[i][k] + matrix_a[i][j] * matrix_b[j][k];
      }
      endtime   = MPI_Wtime();
    MPI_Send(&matrix_c[i][k], ms*ms, MPI_INT, 0, 0, MPI_COMM_WORLD);
    //end of multiplication

    //Display area
    if(myrank == 0)
    {
        //MPI_Recv(matrix_c, ms*ms, MPI_INT, 0, 0, MPI_COMM_WORLD, &status);
        printf("\nRESULT: matrix c \n");
        printf("----------------\n");
         for (i=0; i<ms; i++)
         {
          printf("\n");
          for (k=0; k<ms; k++)
           printf("%3d ", matrix_c[i][k]);
         }
         printf("\n\nParellel Time %f seconds\n",endtime-starttime);
    }
  printf ("\n");




    return 0;
    MPI_Finalize();
}

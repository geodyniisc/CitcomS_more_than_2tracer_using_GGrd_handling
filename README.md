### This code is modified by Debanjan Pal to initialize more than 2 tracers using GGrd

similar to the previous version, but this version is very useful when one want initialize more than 2 tracers in a single layer.

#  1) Append GGrd_handling.c 

### for intrdoucing at similar depth


* new modification by Debanjan Pal */

	E->trace.extraq[j][0][kk]= round(indbl);

      }else{
  
	/* below */

	E->trace.extraq[j][0][kk] = 0.0;

      }
    }
  }

  ### for intrdoucing at different depths

  changes in GGrd_Handling.c
 

if(!E->control.ggrd_comp_smooth){

	  /* limit to 0 or 1 */
   
	  if(indbl < .5)
   
	    indbl = 1.0;
     
	else if(rad > 0.98430 && indbl > .5 && indbl < 1.5) /*Modified by Debanjan Pal to include more than 2 tracers, apprenend according to ggrd file*/
 
		indbl=0.0;
  
	else if(indbl > 1.5)
 
	indbl=2.0;
 
}

	E->trace.extraq[j][0][kk]= indbl;
 
      }else{
      
	/* below */
 
	E->trace.extraq[j][0][kk] = 0.0;
 
      }
    }
  }
#################################################
 Here in this example, 0 is ambient, 1 is weak boundary (0-100km) and 2 is Craton (0-300km)

 The weak boundaries are introduced using this part:

 else if(rad > 0.98430 && indbl > .5 && indbl < 1.5) /*Modified by Debanjan Pal to include more than 2 tracers, apprenend according to ggrd file*/
		indbl=0.0;

 here rad is radius and 0.98430 is 100 km {(1-0.98430)*6370=100km}

# 2) Append Tracer_steup.c


#ifdef USE_GGRD

            case 1:
            
	    case 99:		/* will override restart */
  
	      /* from grid in top n materials, this will override
		 the checkpoint input */
   
	      input_string("ictracer_grd_file",E->trace.ggrd_file,"",m); /* file from which to read */
       
	      input_int("ictracer_grd_layers",&(E->trace.ggrd_layers),"2",m); /* 

									      >0 : which top layers to use, layer <= ictracer_grd_layers
               
									      <0 : only use one layer layer == -ictracer_grd_layers

		/* Modified by Debanjan Pal to allocate memory to trace.nflavors*/		
  
		E->trace.z_interface = (double*) malloc((E->trace.nflavors-1)
                                                        *sizeof(double));
                                                        
                for(i=0; i<E->trace.nflavors-1; i++)
                
                    E->trace.z_interface[i] = 0.7;

                input_double_vector("z_interface", E->trace.nflavors-1,

              
                                    E->trace.z_interface, m);
	      break;
	      
#endif


  

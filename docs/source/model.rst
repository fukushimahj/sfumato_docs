model.hppの設定
=====

.. _model.hppの設定:
------------ 

主に計算の設定は、 ``model.hpp`` で行う.

.. code-block:: console
  
  /***************************************************************
   * header file for model
   * *************************************************************/
  
  #ifndef MODEL_H_
  #define MODEL_H_
  
  
  #define MODEL_NAME      "GMC"          // model name
  
  /*------------------------*/
  /*       GPU TYPES        */
  /*------------------------*/
  #define NVIDIA_GPU_ON  0
  #define AMD_GPU_ON     1
  
  #define GPU_TYPE_USED   NVIDIA_GPU_ON
  
  /*------------------------*/
  /*   SYSTEM OPTION        */
  /*------------------------*/
  #define NPE 2 // number of device
  
  
  // MPI PARALLEL
  #define PARALLEL_MPI      /* MPI */
  
  // openmp
  #define PARALLEL_OMP      /* OMP */
  #define OMP_NUM_THREADS_SINK  10    // number of thread for sink
  
  /*------------------------*/
  /*      GPU check         */
  /*------------------------*/
  #define CHECK_GPU               YES
  #define OUTPUT_GPUPROP          YES
  
  /*------------------------*/
  /*  Memory optionas       */
  /*------------------------*/
  #define MEMORY_ON_HOST
  
  /*------------------------*/
  /*     DEBUG              */
  /*------------------------*/
  #define KERNEL_DEBUG_MODE       OFF
  
  /*------------------------*/
  /*    option for MPI      */
  /*------------------------*/
  #define PRIMARY_RANK   0 // primary rank
  
  #define FL4OPENMPI     0
  #define FL4MVAPICH2    1
  #define FL4INTEL       2
  
  #define MPI_OPTION   FL4OPENMPI
  #define PACKARR_MANAGED           // 転送用のデータはcudaMallocManagedでホスト上に確保される
  
  /* ---------------------- */
  /*          IO            */
  /* ---------------------- */
  #define BLOCKING_IO            ON
  #define OUTPUT_INTERMEDIATE_FILE 
  #define STEP_INTERMEDIATE_FILE   2000000
  #define OUTPUT_DATA_CONSTANT_TIME
  
  /* ---------------------- */
  /*    pinned memory       */
  /* ---------------------- */
  //#define WITHOUT_PINNED_MEMORY
  
  
  /*------------------------*/
  /*  option for timestep   */
  /*------------------------*/
  //#define SINGLE_STEP
  
  /*------------------------*/
  /*        grid            */
  /*------------------------*/
  #define N_GHOST_CELL 2        // number of ghost cell
  
  #define Ni           8        // grid cell number
  #define Nj           8
  #define Nk           8
  
  #define NGI_BASE     8 
  #define NGJ_BASE     8 
  #define NGK_BASE     8 
  
  #define NL           15       // maximum level of refinement
  
  /* ---------------------- */
  /*        timestep        */
  /* ---------------------- */
  #define CFL_HYDRO    0.7
  
  /* ---------------------- */
  /*         hydro          */
  /* ---------------------- */
  #define FLUX_ROEM2             // use numerical flux RoeM2
  
  /*------------------------*/
  /*  boundary condition    */
  /*------------------------*/
  #define bd_cond_OUTFLOW     0
  #define bd_cond_PERIODIC    1
  #define BDCONDITION         bd_cond_OUTFLOW
  //#define ZERO_AVERAGE_DENSITY   // used in fmg
  
  /*------------------------*/
  /*  eos solver            */
  /*------------------------*/
  #define EOS_ADIABATIC_KIM   0
  #define EOS_HLLC_minus      1
  #define EOS_SOLVER EOS_HLLC_minus
  
  /*------------------------*/
  /*  refinement            */
  /*------------------------*/
  #define REFINEMENT          ON
  #define REFINEMENT_NESTED   ON
  #define REFINEMENT_JEANS    ON
  
  #define LMAX0_NEST          1        // level of nsted grid
  #define JEANS_CONST         8       // cell number to resolve a jeans length
  
  #define MERGE_KYOUDAI_MAGO           // if this macro is defined, existence of ancle grid is ensured.
  #define REFINE_NANAME                // if this macro is defined, diagonal grid is also refined
  
  /*------------------------*/
  /*  self gravity          */
  /*------------------------*/
  #define SELF_GRAVITY        ON
  
  /* -----------------------*/
  /* boundary condition of MG*/
  /* -----------------------*/
  #define FMG_BDC_SPHERE      0
  #define FMG_BDC_PERIODIC    1
  
  #if BDCONDITION == bd_cond_PERIODIC
  #define FMG_BDCONDITION FMG_BDC_PERIODIC
  #else
  #define FMG_BDCONDITION FMG_BDC_SPHERE
  #endif
  
  
  
  /* ---------------------- */
  /* rescue options         */
  /* ---------------------- */
  #define RESCUE_NANINF       ON
  #define RESCUE_FLOOR        ON
  
  /* -----------------------*/
  /*   count time*/
  /* -----------------------*/
  #define OUTPUT_CALTIME      ON
  
  /*------------------------*/
  /*  chemistry solver      */
  /*------------------------*/
  #define CHEMSOLVER          ON
  #define OUTPUT_RHOTPLANE    ON
  #define CHEM_COSMIC_RAY     ON
  #define CHEMISTRY_EXPLICIT_SOLVER
  #define CHEMISTRY_ON_CPU
  
  namespace MODEL_CHEMP{
    constexpr REAL zred   = 0.e0;       // redshift
    constexpr REAL metal  = 1.e0;       // metallicity
  
    constexpr REAL RHOT_Tmin    = 2.7e0;   
    constexpr REAL RHOT_Tmax    = 3.e4;   
    constexpr REAL RHOT_nHmin   = 1.e0;
    constexpr REAL RHOT_nHmax   = 1.e12;
    constexpr INT32 RHOT_bin    = 100;
    constexpr INT32 ngrid       = 1024*16;  // 一度に計算するgridの数
  #if CHEM_COSMIC_RAY == ON
    constexpr REAL cosmicray_iorate = 1.e-17;
  #endif
  };
  
  /* ------------------------ */
  /*   radiation transfer     */
  /* ------------------------ */
  #define RADTR_M1closure     ON
  
  #define EUV_RADTRM1         ON
  #define FUV_RADTRM1         ON
  #define IR_RADTRM1          ON
  
  #define RADTR_HLL           0
  #define RADTR_GLF           1
  #define RADTR_SOLVER        RADTR_HLL
  #define REDUCED_LIGHTSPEED  1.e-4
  
  #if RADTR_M1closure == OFF
    #define WITHOUT_RADTR  
  #endif
  
  /* ------------------------ */
  /*    radiation sources     */
  /* ------------------------ */
  #define FOL_RADIATION_SOURCES "prostfit/Z0"
  #define USE_REALTIME_ACCRETION_RATE4STELLAR_LUMI
  #define PHOTON_INJECTION_IR_AT_SPLEVEL
  
  /*-----------------------*/
  /*   model parameters    */
  /*-----------------------*/
  #include "unit.hpp"
  namespace modelParameter{
  
    // simulation setup
    constexpr REAL  Elapselimit   = 22.5e0;                   // [h], maximum comutational time in hour (<0: ignored this option)
    constexpr REAL  Vmax          = 5.e2/unit::Unit_kms;    // [noD], maximum velocity
    constexpr REAL  Tmax          = 1.e5;                   // [Kelvin], maximum temperatur
    constexpr REAL  Tmin          = 1.e0;                   // [Kelvin], manimum temperatur
    constexpr REAL  Nmin          = 1.e-3;                  // [cm^-3], density floor as the number density of hydrogen
    constexpr REAL  Tlast         = 1.e7/unit::Unit_yr;     // [noD], maximum time of calculation (<0: maximum simulation time is ignored as the end condition of simulation)
    constexpr INT64 Step_max      = -486250;                   // maximum step (<0: maximum step is ignored)
    constexpr REAL  Op4yHe        = 1.008e0 + 4.004e0*unit::cgs_yHe;  // mean molecular weight
  #if CHEMSOLVER == OFF
    constexpr REAL gamma  = 5.e0/3.e0;                            // polytropic inde x
  #endif
    constexpr REAL RHOMINcu       =  Nmin*Op4yHe*unit::cgs_amu/unit::Unit_rho;      // density floor in code unit
    constexpr REAL Vmax2          =  Vmax*Vmax;    // maximum velocity in code [noD]
  
    // Turbulent sphere models
    constexpr REAL Rcl            = 3.e0*unit::cgs_pc/unit::Unit_l;   // radius of cloud 
    constexpr REAL Boxsize        = 4.e0*Rcl;                         // Boxsize
    constexpr REAL Mcl            = 1.e3*unit::cgs_msun/unit::Unit_m; // cloud mass
    constexpr REAL alpha          = 1.e0;                             // virial parameter
    constexpr REAL T0             = 10.e0;                  // gas temperature     [K]
    constexpr REAL cs_HI          = unit_func::sqrt(unit::cgs_kb*T0/(unit::cgs_mh*(1.e0+4.e0*unit::cgs_yHe)/(1.e0+unit::cgs_yHe)))/unit::Unit_v;   // isothermal sound speed of HI
    constexpr REAL rho_ext        = 1.e-4;                            // gas density outsize cloud 
  
    // data output
    constexpr INT32 Dstep         = 1000000;                      // data output interval
    constexpr INT32 ncout         = 128;                     // number of cell for output
  #ifdef OUTPUT_INTERMEDIATE_FILE 
    constexpr INT32 Dstep_INTF    = STEP_INTERMEDIATE_FILE;   // intermidiate file
  #endif
  #ifdef OUTPUT_DATA_CONSTANT_TIME
    constexpr REAL Dtime_ODCT     = 5.e3*unit::cgs_yr/unit::Unit_t;  // 
  #endif
  };
  
  // turbulent file
  #define TURB_FILE   "turb.d"
  
  // physical value for simulation
  namespace PHYSVAL_INI{
  };
  
  
  
  /*------------------------*/
  /*   sink particle        */
  /*------------------------*/
  #define SINKPARTICLE          ON
  
  #define SINKTYPE_STAR         0
  #define SINKTYPE_SC           1
  
  #define SINKTYPE              SINKTYPE_STAR   // type of sink particle
  #define SPCREATION            ON              // creation of sink particle
  #define PARTICLECREATION_IN_BOUNDARY          // Particle can be created in boundary planes otherwise particle creation is forbidden in the boundary plane
  #define SPACCRETION           ON              // accretion
  #define SPADVECTION           ON              // advection
  #define SOFTNING_P2P_INCLUDE  ON              // softning of sp gravity
  #define SP_GRAVITY_CIC        ON              // use CIC method for sink gravity
  #define SPMERGE               ON
  
  // parameters for sink
  namespace SinkParameter{
  
  #ifdef PARALLEL_OMP
    constexpr INT32 omp_num = OMP_NUM_THREADS_SINK;      // number of thread for sink
  #endif
  
  #if SINKTYPE == SINKTYPE_STAR
    constexpr REAL spNcr = 1.e10;         // [cm^-3] threshold gas number density for sink creation
    constexpr REAL spCs  = 2.4376e4;         // [cm/s]  sound speed for gas at nH=1e12cm^-3
    constexpr REAL spRadius_lamJ = 0.5e0;  // sink 粒子はjeans長何個分解像するか
    constexpr REAL spRadius_cell = JEANS_CONST;  // sink 半径は最深セル何個分か
  #endif
  
  #if SINKTYPE == SINKTYPE_SC
    constexpr REAL spCs   = 7.7e4;         // [cm/s]  sound speed for gas at nH=1e12cm^-3
    constexpr INT32 Nsink = 1;              // number of cell radius
    constexpr REAL spRadius_cell = 2*Nsink; // sink 半径は最深セル何個分か
  #endif
  
    constexpr INT32 N_SubCell = 8;                      // number of cell in subcycle
    constexpr INT32 N_sp      = 2*int(spRadius_cell)+1; 
    constexpr REAL sp_CGS     = 0.1e0;                  // parameter for subcycle of sinkparticle
    constexpr REAL dt_acc     = 1.e3/unit::Unit_yr;     // duration time for update date subdisk [noD]]
    constexpr REAL rdt_acc    = 0.05e0;                  // time interval of update of stellar evolution in tacc or tkh units
  
    constexpr INT32 splog_skip = 10000;                   // timestep for output
    constexpr REAL  mdot_floor  = 5.e-7*unit::cgs_msun/unit::cgs_yr/unit::Unit_m*unit::Unit_t; // [g/s] => [noD]
    constexpr REAL  Jeans_merge = 32.e0;                  // merge for refinement  
  };
  
  
  /*------------------------*/
  /*  label for variable    */
  /*------------------------*/
  enum{
    MRHO,
    MVX,
    MVY,
    MVZ,
    MP,
  #if SELF_GRAVITY == ON
    MPSI,
    MGX,
    MGY,
    MGZ,
  #endif
  #if CHEMSOLVER == ON
    MCHEM_H2,   // ここを変更する場合, eos_dも変更必要
    MCHEM_EL,
    MCHEM_HI,
    MCHEM_HII,
  #endif
  // causion ! 輻射についてのラベルは最後に並べる(timestep_d.cuでのindexの都合上, grid.cuhのnm_flux_nradを参照)
  #if RADTR_M1closure == ON && EUV_RADTRM1 == ON    
    MER_EUV,
    MFRX_EUV,
    MFRY_EUV,
    MFRZ_EUV,
  #endif
  #if RADTR_M1closure == ON && FUV_RADTRM1 == ON
    MER_FUV,
    MFRX_FUV,
    MFRY_FUV,
    MFRZ_FUV,
  #endif
  #if RADTR_M1closure == ON && IR_RADTRM1 == ON
    MER_IR,
    MFRX_IR,
    MFRY_IR,
    MFRZ_IR,
  #endif
  #if CHEMSOLVER == ON && defined(CHEMISTRY_EXPLICIT_SOLVER)
    MDUST,
  #endif
    NM
  };
  /* causion !! order of variables is need to be consistent with eos_d.cu */
  
  #if defined(CHEMISTRY_EXPLICIT_SOLVER) && defined(CHEMISTRY_ON_CPU) 
  #ifndef PARALLEL_OMP
    #define PARALLEL_OMP
  #endif
  #ifndef  MEMORY_ON_HOST
    #define MEMORY_ON_HOST
  #endif
  #endif
  
  #endif


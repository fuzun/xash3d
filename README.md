# xash3D-strobe

![sample-strobe-screenshot](https://vgy.me/Tdf4gZ.png)

### How to hook a StrobeAPI derived work (StrobeAPI implementation) into xash3d ?

 - in **cl_view.c** *V_PostRender* function, this part can be modified:
   
       #ifdef STROBE_ENABLED
       		// if ( STROBE_TEMPLATE )
       		//  STROBE_TEMPLATE->STROBE_IMPL_FUNC_DEBUGHANDLER( &STROBE_TEMPLATE );
       		if ( STROBE_CORE )
       			STROBE_CORE->STROBE_IMPL_FUNC_DEBUGHANDLER( &STROBE_CORE );
       #endif 
      Debug handling function (printing informative strobe debugging text output from StrobeAPI) of your Strobe implementation can be called here.

 - in **gl_rmain.c** *R_EndFrame* function, this part can be modified:
   
       #ifdef STROBE_ENABLED
       // StrobeAPI.Invoker( STROBE_INVOKE(STROBE_TEMPLATE) );
       StrobeAPI.Invoker( STROBE_INVOKE(STROBE_CORE) );
       #else
  - Check **r_strobe_template.c.TEMPLATE** and **r_strobe_template.h.TEMPLATE** files. They are templates for creating a new StrobeAPI implementation. They can be used as a base and for actual implementation Strobe-Core (r_strobe_core.h and r_strobe_core.c) can be inspected.

### How StrobeAPI and StrobeAPI implementations exposed to xash3d ?

 - A structure which contains console variables and Invoker, Constructor and Destructor functions are exposed through "StrobeAPI" instance of the structure.
 - R_InitStrobeAPI( ) from StrobeAPI for registering console variables is exposed.
 - Constructor, destructor, reinit, main functions from StrobeAPI implementations are exposed while being guarded by StrobeAPI macros.
 - Bunch of macros starting with STROBE or _STROBE from StrobeAPI are exposed.
 - Bunch of structures starting with STROBE or StrobeAPI_ from StrobeAPI are exposed.
 - An implementation structure and its instance from StrobeAPI implementation are exposed while being guarded by StrobeAPI macros.
 - StrobeAPI and StrobeAPI implementation generic functions and private variables etc. are NOT exposed.
 - Contents of r_strobe_api_protected_.h are not exposed.
 - If STROBE_DISABLED is defined, nothing is exposed.

### Console variables

 - **r_strobe** : For setting Strobe method. 1: NORMAL - BLACK , 2 : NORMAL - BLACK - BLACK, -3: BLACK - BLACK - BLACK - NORMAL . Set 0 to disable strobing.
 - **r_strobe_swapinterval** : For setting phase swap interval of Strobing. (in seconds). May cause image retention if set to 0 in some monitors (0 disables phase swapping).
 - **r_strobe_debug** : For informing StrobeAPI instance to do something with StrobeAPI informative debug output text. Strobe-Core implementation shows the output text in right-up corner of the screen. (0 or 1)
 - **r_strobe_cooldown** : When standard deviation of collected FPS values in a certain time period exceeds the determined limit, strobing gets disabled for a time period. This cvar sets the time period of how long strobing is disabled when there is instability. Should be set 0 to disable instability induced strobing cooldown.
 - deviation limit (not implemented): Standard deviation limit of FPS values collected in a time period.
 - count of collected fps values (not implemented): How many FPS values should be collected before calculating standard deviation.

These variables are common in all StrobeAPI implementations because they are defined in StrobeAPI. In future these variables should be defined in implementations instead of StrobeAPI.

### License

The library is licensed under GPLv3 license, see [COPYING](https://github.com/FWGS/xash3d/blob/master/COPYING) for details.
CMakeLists.txt files are licensed under MIT license, you will find it's text
in every CMakeLists.txt header.

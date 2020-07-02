# How-to-Write-Systick-Timer-CMSIS-Style

How to Write Systick Timer CMSIS Style     July 1, 2020

I appreciate comments. Shoot me an email at noel_s_cruz@yahoo.com!

In previous posts, we surveyed and examined steps toward writing a Systick timer using
register programming. We wrote in ARM assembly and in C. Today we show the CMSIS style.

By the way, the previous post is: https://github.com/noey2020/How-to-Write-SysTick-Handler-for-STM32.

CMSIS provides us core and device header and C files. In particular, SysTickConfig is 
coded from lines 1815-1854  in core_cm3.h. Contrast the CMSIS code to our register 
programming in assembly and C, and we will find that they are very much similar.

Some code snippets from core_cm3.h:

__STATIC_INLINE uint32_t SysTick_Config(uint32_t ticks)
{

  if ((ticks - 1UL) > SysTick_LOAD_RELOAD_Msk)
  {
  
    return (1UL);                                                   /* Reload value impossible */
    
  }

  SysTick->LOAD  = (uint32_t)(ticks - 1UL);                         /* set reload register */
  
  NVIC_SetPriority (SysTick_IRQn, (1UL << __NVIC_PRIO_BITS) - 1UL); /* set Priority for Systick Interrupt */
  
  SysTick->VAL   = 0UL;                                             /* Load the SysTick Counter Value */
  
  SysTick->CTRL  = SysTick_CTRL_CLKSOURCE_Msk |
  
                   SysTick_CTRL_TICKINT_Msk   |
                   
                   SysTick_CTRL_ENABLE_Msk;                         /* Enable SysTick IRQ and SysTick Timer */
                   
  return (0UL);                                                     /* Function successful */
}

Since the legwork has been done by CMSIS, it narrows the coding to just a single line which is:

uint32_t SysTick_Config (uint32_t ticks);

Also, it is very helpful to check the docs website https://www.keil.com/pack/doc/CMSIS/Core/html/group__SysTick__gr.html.

Attached are the files to experiment and have a proof of concept.

I appreciate comments. Shoot me an email at noel_s_cruz@yahoo.com!

Don't forget the newline and happy coding!

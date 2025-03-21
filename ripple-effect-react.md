## how to create ripple effect on react?
Here I will show how to create custom ripple effect with  without any package, just put `className` anywhere you want to add ripple effect. <br/>
For example the `className` is `ripple-effect` just put this as like `<button className='ripple-effect'>click</button>`, that's it
<br/>
<br/>

### let's jump on code 🚀

#### Step 1. create a custom hooks `useRippleEffect.ts`
  ```js
  import { useEffect } from 'react';

  export const useRippleEffect = () => {
    useEffect(() => {
      const handleClick = (e: MouseEvent) => {
        // Find the nearest ripple-enabled element
        const target = (e.target as HTMLElement)?.closest('.ripple-effect') as HTMLElement | null;
        
        if (!target) return;
  
        const rect = target.getBoundingClientRect();
        const size = Math.max(rect.width, rect.height);
        const x = e.clientX - rect.left - size / 2;
        const y = e.clientY - rect.top - size / 2;

        //Dynamically creates a <span> at the click point with a growing/fading animation.
        const ripple = document.createElement('span');
        ripple.className = 'ripple-effect-elm';
        ripple.style.width = ripple.style.height = `${size}px`;
        ripple.style.left = `${x}px`;
        ripple.style.top = `${y}px`;
  
        target.appendChild(ripple);
  
        // Remove after animation
        setTimeout(() => {
          ripple.remove();
        }, 600);
      };
  
      document.addEventListener('click', handleClick);
      return () => document.removeEventListener('click', handleClick);
    }, []);
  };

  ```
#### Step 2. Add classes on `global.css`
```css
  .ripple-effect{
    position: relative;
    overflow: hidden;
  }
  .ripple-effect-elm {
    position: absolute;
    background: rgba(0, 0, 0, 0.1);
    border-radius: 50%;
    transform: scale(0);
    animation: ripple-animation 0.6s linear;
    pointer-events: none;
    z-index: 1;
  }

  @keyframes ripple-animation {
  to {
    transform: scale(4);
    opacity: 0;
  }
}

```
#### Step 3. import `useRippleEffect.js` on top level component 
### use the `ripple-effect` className any of the element 👍
  

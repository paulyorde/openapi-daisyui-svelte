<!-- MyElement.svelte -->
<script>
    import { onMount } from 'svelte';
	import { createEventDispatcher } from 'svelte';

  export let id;
  // export let registerComponent;
  let instance;

  function setInstance(ref) {
    instance = ref;
    registerComponent(id, instance);
  }

  

  const dispatch = createEventDispatcher();

    export let px = 100;
    export let inputValue = ''
    export let chatAssistant = 'welcome to chat mediator'

    let element = null
    let myDiv
    let elementData = {
      type: 'div',
      text: chatAssistant,
      margintop: `${px}px`
    };

    function appendContent() {
      const target = document.getElementById('myDiv');
      target.style.marginTop = elementData.margintop
      target.innerHTML += chatAssistant;
    }



    // onMount(() => {
    //   console.log('ele comp mounted')
    //   element = document.createElement(elementData.type);
    //   element.textContent = elementData.text;
    //   element.style.marginTop = elementData.margintop
    //   appendContent(element)
    //   // document.body.appendChild(element);

    //   dispatch('elementCreated', { element });
	  // });

    // update function 
    // $: {
		// 	if (element) {
		// 		element.parentNode.replaceChild(element, element);
		// 		element = document.createElement(elementData.type);
		// 		element.textContent = elementData.text;
		// 		element.style.marginTop = elementData.margintop
    //     appendContent(element)

		// 		// document.body.appendChild(element);

		// 		dispatch('elementCreated', { element });
		// 	}
		// }
  </script>

<!-- <svelte:component this={setInstance} /> -->
  
  <!-- <svelte:element this={elementType}>{elementText}</svelte:element> -->

  <div id="myDiv"></div>

  <!-- <div bind:this={element}></div> -->


  <div class="chat chat-start" style="margin-top: {`${px}px`};">
    <div class="chat-image avatar">
      <div class="w-10 rounded-full">
        <!-- svelte-ignore a11y-missing-attribute -->
        <!-- <img src="/images/stock/photo-1534528741775-53994a69daeb.jpg" /> -->
      </div>
    </div>
    <div class="chat-header">
      Darth User
      <time class="text-xs opacity-50">12:45</time>
    </div>
    <div class="chat-bubble">{inputValue}</div>
    <div class="chat-footer opacity-50">
      Delivered
    </div>
  </div>
  <div class="chat chat-end">
    <div class="chat-image avatar">
      <div class="w-10 rounded-full">
        <!-- svelte-ignore a11y-missing-attribute -->
        <!-- <img src="/images/stock/photo-1534528741775-53994a69daeb.jpg" /> -->
      </div>
    </div>
    <div class="chat-header">
      AnakinSky Bot
      <time class="text-xs opacity-50">12:46</time>
    </div>
    <div class="chat-bubble">{chatAssistant}</div>
    <div class="chat-footer opacity-50">
      Seen at 12:46
    </div>
  </div>
  
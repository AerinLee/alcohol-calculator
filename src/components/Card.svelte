<script>
    import { createEventDispatcher } from 'svelte'

    export let title;
    export let emoji;

    const dispatch = createEventDispatcher();

    function selectCard(el){

        if(document.getElementsByClassName("selected").length > 0){
            document.getElementsByClassName("selected")[0].classList.remove('selected')
        }

        el.currentTarget.classList.add('selected')

        dispatch('message', {
			title: title
		});
    }

</script>

<div class="card-wrap" >
    <div class="card" on:click="{selectCard}">
        <div class="card-body">
            <div class="card-title">{title}</div>
            <div class="emoji">{emoji}</div>
        </div>
    </div>

</div>

<style>
    .card-wrap {
        flex : 0 0 auto;
        margin: 0 1rem;
        cursor: pointer;
        
    }

    


    .card {
        box-shadow: 0 0.15rem 1.75rem 0 rgba(33, 40, 50, 0.15);  
        height: 100%;
        width: 10rem;
        position: relative;
        flex-direction: column;
        word-wrap: break-word;
        border: 2px solid salmon;
        border-radius: 0.35rem;
        
    }
    
    .card:not(.selected){
        border: 1px solid rgba(33, 40, 50, 0.125);
        border-radius: 0.35rem;
    }

    .card-body {
        flex: 1 1 auto;
        padding: 1.35rem 1.35rem
    }

    .card-title {
        font-size: 1.4rem;
        font-family: 'GowunBatang-Bold'
    }

    .emoji {
        font-size: 3rem;
    }

</style>
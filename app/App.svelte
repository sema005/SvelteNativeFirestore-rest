<script>
 import FirestoreParser from 'firestore-parser'
 let items =[]
 const baseUrl = 'https://firestore.googleapis.com/v1/'
 const productsUrl = 
 baseUrl + 'projects/svelte-native/databases/(default)/documents/products'
const getProducts = () => {
    fetch(productsUrl)
    .then( response => response.json() )
        .then( json => FirestoreParser(json) )
            .then( parsed => {
                items = parsed.documents
            })
    .catch( error => console.log(error) )
}
getProducts()
</script>

<page>
    <actionBar title="Svelte Firestore REST" />
    <scrollView class='main'>
        <stackLayout>
            {#each items as item}
                <flexboxLayout class='product'>
                    <image src={item.fields.image} stretch='aspectFit' />
                    <stackLayout>                
                        <label class='h1' text={item.fields.title} />
                    </stackLayout>
                </flexboxLayout>
            {:else}
                <activityIndicator busy={true} />
            {/each}
        </stackLayout>
    </scrollView>
</page>


<style>
    .main{
        background-color:#aaa;
    }
    .product{
        margin: 8 8 0 8;
        background-color:lightgray;
        border-radius:8;
        padding:16;
    }
    .product > image{
        height:80;
        margin-right:16;
    }
</style>
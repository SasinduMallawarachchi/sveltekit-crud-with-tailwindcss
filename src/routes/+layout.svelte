
<script>
    import "../app.css";
    import {onMount} from 'svelte'
    import {auth, db} from '../lib/firebase/firebase'
    import { getDoc, doc, setDoc } from 'firebase/firestore';
    import {authStore} from '../store/store';

    const nonAuthRoutes = ['/', 'product']
    
    onMount(()=>{
        console.log('Mounting')
        const unsubscribe = auth.onAuthStateChanged(async user =>{
            const currentPath = window.location.pathname

            if(!user && !nonAuthRoutes.includes(currentPath)){
                window.location.href="/";
                return
            }
            if(user && currentPath == "/"){
                window.location.href = "dashboard";
                return;
            }

            if(!user){
                return;
            }

            let dataToSetStore;
            const docRef = doc(db, 'users', user.uid);
            const docSnap = await getDoc(docRef);
            if(!docSnap.exists()){
                const userRef = doc(db, "user", user.uid);
                dataToSetStore = {
                    email: user.email,
                    todos: [],
                }
                await setDoc(userRef, dataToSetStore, {merge: true});
            }else{
                const userData = docSnap.data();
                dataToSetStore = userData;
            }
            authStore.update((curr)=>{
                return{
                    user,
                    data: dataToSetStore,
                    loading: false,
                };
            });
        });
    });
</script>


<div class="relative flex h-screen text-black max-h-1000 bg-gradient-to-r from-cyan-100 to-blue-100">
    <slot/>
</div>


<script>
    import NavBar from "../../../components/NavBar.svelte";
    import {goto} from '$app/navigation'
  import { onMount } from "svelte";
    const handlesubmit = async () => {
      // Retrieve the values of the input fields
      const carType = document.getElementById('carType').value;
      const carModel = document.getElementById('carModel').value;
      const carName = document.getElementById('carName').value;
      const imageUrl = document.getElementById('imageUrl').value;
  
      try {
        const res=await fetch('https://carstore-backend-jv67.vercel.app/addcar',{
          method:"POST",
          headers:{
            "Content-Type":"application/json"
          },
          body:JSON.stringify({type:carType,name:carName,model:carModel,car_info:{imageUrl,dealer_id:localStorage.getItem('logindata')}})
        })
        if(res.ok){
          alert("car added");
        }else{
          throw new Error("not able to add car")
        }
        const data=await res.json();
        console.log(data);

      } catch (error) {
        console.log(error);
      }
    }
    onMount(async()=>{
      if(localStorage.getItem('logintype')!='dealer'){
        goto('/');
      }
    })
  </script>
  
  <NavBar />
  
  <div class="w-full flex flex-col items-center justify-center p-3">
      <p class="font-bold text-3xl p-3">Add New Car</p>
     <div class="w-1/2 p-2 gap-5 flex flex-col justify-center items-center">
          <input type="text" id="carType" placeholder="Car Type" class="input input-bordered w-full max-w-xs" />
          <input type="text" id="carModel" placeholder="Car Model" class="input input-bordered w-full max-w-xs" />
          <input type="text" id="carName" placeholder="Car Name" class="input input-bordered w-full max-w-xs" />
          <input type="text" id="imageUrl" placeholder="Image URL" class="input input-bordered w-full max-w-xs" />
          <button class="btn btn-success max-w-xs w-full" on:click={handlesubmit}>Submit</button>
      </div> 
  </div>
  
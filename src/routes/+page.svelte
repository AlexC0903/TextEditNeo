<script lang="ts">
  import { enhance } from "$app/forms";
  import { invalidate } from "$app/navigation";

  let name = "Alex";

  import { Button } from "$lib/components/ui/button";
  import { Card, CardContent, CardHeader } from "$lib/components/ui/card";
  import { Input } from "$lib/components/ui/input";
  import { Label } from "$lib/components/ui/label";
  const { data } = $props();
  const { userProfile } = data;

  let firstName = $state("");
  let lastName = $state("");
  let email = $state("");

  $effect(() => {
    if (userProfile) {
      firstName = userProfile.firstName;
      lastName = userProfile.lastName;
      email = userProfile.email;
    }
  });
</script>

{#if userProfile}
  <Card>
    <CardHeader>Manage your profile</CardHeader>
    <CardContent>
      <form
        method="post"
        use:enhance={({ formData }) => {
          formData.set("firstName", firstName);
          formData.set("lastName", lastName);
          formData.set("email", email);
          return ({ result }) => {
            if (result.type === "success") {
              invalidate("/");
              alert("UPDATED");
            } else {
              alert("ERROR");
            }
          };
        }}
      >
        <div>
          <Label>Email</Label>
          <Input bind:value={email} />
        </div>
        <div>
          <Label>First Name</Label>
          <Input bind:value={firstName} />
        </div>
        <div>
          <Label>Last Name</Label>
          <Input bind:value={lastName} />
        </div>
        <div>
          <Button type="submit">Update</Button>
        </div>
      </form>
    </CardContent>
  </Card>
  <Button href="/auth/logout">Logout</Button>
{:else}
  <Button href="/auth/login">Login to the Site</Button>
{/if}

<!-- <main> -->
<!--   <div class="h-screen flex flex-col"> -->
<!--     <!-- Top half containing two divs --> -->
<!--     <div class="flex flex-1"> -->
<!--       <!-- Top-left div --> -->
<!--       <div class="flex-1 bg-gray-200 relative"> -->
<!--         <!-- Wrapper for centered title --> -->
<!--         <div class="absolute inset-0 flex items-start my-10 justify-center"> -->
<!--           <h2 class="text-xl font-semibold">Get Started</h2> -->
<!--         </div> -->
<!--         <div class="flex-1 flex justify-center items-center inset-0 h-full"> -->
<!--           <div class="mx-10"> -->
<!--             <span>New Doc</span> -->
<!--           </div> -->
<!--           <div class="mx-10"> -->
<!--             <span>Open Doc</span> -->
<!--           </div> -->
<!--         </div> -->
<!--       </div> -->
<!---->
<!--       <!-- Top-right div with centered h1 --> -->
<!--       <div class="flex-1 flex items-center justify-center"> -->
<!--         <h1 class="text-center text-6xl font-bold">Hello, {name}!</h1> -->
<!--       </div> -->
<!--     </div> -->
<!---->
<!--     <!-- Bottom div --> -->
<!--     <div class="flex-1 bg-gray-300 justify-center flex"> -->
<!--       <div class="abosolute inset-0 flex items-start my-10 justify-center"> -->
<!--         <h1>Documents</h1> -->
<!--       </div> -->
<!--     </div> -->
<!--   </div> -->
<!-- </main> -->

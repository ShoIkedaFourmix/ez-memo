<template>
  <div class="elements">
    <div class="container-fluid">
      <div class="row">
        <div class="col-6">
          <h1>Elements</h1>
          <h2>Elements</h2>
          <h3>Elements</h3>
          <h4>Elements</h4>
          <h5>Elements</h5>
        </div>
        <div class="col-6">
          <!-- Button trigger modal -->
          <button type="button" class="btn btn-primary m-4" data-bs-toggle="modal" data-bs-target="#exampleModal">
            <div class="corner left_top"></div>
            <div class="corner left_bottom"></div>
            <div class="corner right_top"></div>
            <div class="corner right_bottom"></div>
            Launch demo modal
          </button>

          <!-- Modal -->
          <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog modal-xl">
              <div class="modal-content">
                <div class="modal-header">
                  <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
                </div>
                <div class="modal-body">
                  Lorem ipsum dolor sit amet, consectetur adipisicing elit. Accusamus, amet cupiditate laboriosam sunt libero aliquam, consequatur alias ducimus adipisci nesciunt odit? Odio tenetur et itaque suscipit atque officiis debitis qui. Lorem ipsum dolor sit amet, consectetur adipisicing elit. Accusamus, amet cupiditate laboriosam sunt libero aliquam, consequatur alias ducimus adipisci nesciunt odit? Odio tenetur et itaque suscipit atque officiis debitis qui. Lorem ipsum dolor sit amet, consectetur adipisicing elit. Accusamus, amet cupiditate laboriosam sunt libero aliquam, consequatur alias ducimus adipisci nesciunt odit? Odio tenetur et itaque suscipit atque officiis debitis qui.
                </div>
                <div class="modal-footer justify-content-center">
                  <button type="button" class="btn btn-primary">
                    <div class="corner left_top"></div>
                    <div class="corner left_bottom"></div>
                    <div class="corner right_top"></div>
                    <div class="corner right_bottom"></div>
                    Save changes
                  </button>
                  <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                    <div class="corner left_top"></div>
                    <div class="corner left_bottom"></div>
                    <div class="corner right_top"></div>
                    <div class="corner right_bottom"></div>
                    Close
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="row mt-4">
        <div class="col-12">
          <button type="button" class="btn btn-primary m-2">
            <div class="corner left_top"></div>
            <div class="corner left_bottom"></div>
            <div class="corner right_top"></div>
            <div class="corner right_bottom"></div>
            Primary
          </button>
          <button type="button" class="btn btn-secondary m-2">
            <div class="corner left_top"></div>
            <div class="corner left_bottom"></div>
            <div class="corner right_top"></div>
            <div class="corner right_bottom"></div>
            Secondary
          </button>
          <button type="button" class="btn btn-success m-2">
            <div class="corner left_top"></div>
            <div class="corner left_bottom"></div>
            <div class="corner right_top"></div>
            <div class="corner right_bottom"></div>
            Success
          </button>
          <button type="button" class="btn btn-danger m-2" disabled>
            <div class="corner left_top"></div>
            <div class="corner left_bottom"></div>
            <div class="corner right_top"></div>
            <div class="corner right_bottom"></div>
            Danger
          </button>
        </div>
      </div>
      <div class="row mt-4">
        <div class="col-6">
          <form>
            <div class="mb-3">
              <label for="exampleInputEmail1" class="form-label">Email address</label>
              <input type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
              <div id="emailHelp" class="form-text">We'll never share your email with anyone else.</div>
            </div>
            <div class="mb-3">
              <label for="exampleInputPassword1" class="form-label">Password</label>
              <input type="password" class="form-control" id="exampleInputPassword1">
            </div>
            <div class="mb-3">
              <label for="exampleFormControlTextarea1" class="form-label">Example textarea</label>
              <textarea class="form-control" id="exampleFormControlTextarea1" rows="3"></textarea>
            </div>
            <div class="mb-3 form-check">
              <input type="checkbox" class="form-check-input" id="exampleCheck1">
              <label class="form-check-label" for="exampleCheck1">Check me out</label>
            </div>

            <div class="form-check">
              <input class="form-check-input" type="radio" name="flexRadioDefault" id="flexRadioDefault1">
              <label class="form-check-label" for="flexRadioDefault1">
                Default radio
              </label>
            </div>
            <div class="form-check">
              <input class="form-check-input" type="radio" name="flexRadioDefault" id="flexRadioDefault2" checked>
              <label class="form-check-label" for="flexRadioDefault2">
                Default checked radio
              </label>
            </div>

            <button type="submit" class="btn btn-primary">
              <div class="corner left_top"></div>
              <div class="corner left_bottom"></div>
              <div class="corner right_top"></div>
              <div class="corner right_bottom"></div>
              Submit
            </button>
          </form>
        </div>
        <div class="col-6">
          <h2>
            A EZMEMO UI PROJECT
          </h2>
          <p>
            Lorem ipsum dolor sit amet, consectetur adipisicing elit. Accusamus, amet cupiditate laboriosam sunt libero aliquam, consequatur alias ducimus adipisci nesciunt odit? Odio tenetur et itaque suscipit atque officiis debitis qui. Lorem ipsum dolor sit amet, consectetur adipisicing elit. Accusamus, amet cupiditate laboriosam sunt libero aliquam, consequatur alias ducimus adipisci nesciunt odit? Odio tenetur et itaque suscipit atque officiis debitis qui. Lorem ipsum dolor sit amet, consectetur adipisicing elit. Accusamus, amet cupiditate laboriosam sunt libero aliquam, consequatur alias ducimus adipisci nesciunt odit? Odio tenetur et itaque suscipit atque officiis debitis qui.
          </p>
          <p class="text-danger">
            Lorem ipsum dolor sit amet, consectetur adipisicing elit. Accusamus, amet cupiditate laboriosam sunt libero aliquam, consequatur alias ducimus adipisci nesciunt odit? Odio tenetur et itaque suscipit atque officiis debitis qui.
          </p>
          <p class="text-success">
            With animations based on SciFi and designs from high technology
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Elements"
}
</script>
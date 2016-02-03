
jQuery(function($) {
  var $loader = $('#snapshot-plugin .loader-inner.ball-grid-pulse');
  var $errorOutput = $('#snapshot-plugin .output.error');

  $('#snapshot-plugin #download-snapshot').on('click', function() {
    window.location.href = document.location.origin + '/static-site.tar';
  });

  /**
   * create new snapshot
   */
  $('#snapshot-plugin #create-snapshot').on('click', function() {
    $errorOutput.hide();
    var data = {
      name: $('#snapshot-name').val(),
      action: 'add_snapshot'
    }

    if(data.name.length === 0) {
      $errorOutput.text('Snapshot name is required');
      $errorOutput.show();
      return;
    }

    if(data.name.length > 200) {
      $errorOutput.text('Snapshot name is too long!');
      $errorOutput.show();
      return;
    }

    $('#snapshot-plugin .loader-inner.ball-grid-pulse').css('display', 'inline');

    $.ajax({
      method: 'POST',
      url: 'admin-ajax.php',
      data: data,
      dataType: 'json'
    }).done(function(response) {
      console.log(response); 
      // console.log(JSON.stringify(response, null, 2));
      var snapshot = response.snapshot;
      var rowId = 'snapshot-' + snapshot.name;

      $('#template-row').clone().prop('id', rowId).appendTo('#available-snapshots > table').show();
      $('#' + rowId + ' td:nth-child(1)').html(snapshot.id);
      $('#' + rowId + ' td:nth-child(2)').html(snapshot.name);
      $('#' + rowId + ' td:nth-child(3)').html(snapshot.creationDate.split(' ')[0]);
      $('#' + rowId + ' td:nth-child(4) a').prop('href', response.snapshot_url).on('click', delete_snapshot);
      $('#' + rowId + ' td:nth-child(5) a').prop('id', 'delete-snapshot-' + snapshot.name).on('click', delete_snapshot);

      $('#available-snapshots').show();
      
    }).fail(function(response) {
      console.log(response);

      $errorOutput.text(response.responseJSON.message);
      $errorOutput.show();

    }).always(function(response) {
      $loader.hide();
    });
  });

  /**
   * delete snapshot
   */
  function delete_snapshot() {
    var data = {
      name: this.id.split('-snapshot-')[1],
      action: 'delete_snapshot'
    }

    $.ajax({
      method: 'POST',
      url: 'admin-ajax.php',
      data: data,
      dataType: 'json'
    }).done(function(response) {
      $('#snapshot-' + data.name).remove();
      if($("#available-snapshots > table tr > td").length < 1) {
        $('#available-snapshots').hide();
      }
      
    }).fail(function(response) {
      console.log(response);

      $errorOutput.text(response.responseJSON.message);
      $errorOutput.show();

    }).always(function(response) {
      $loader.hide();
    }); 
  }

  $('[id^=delete-snapshot-]').on('click', delete_snapshot);
});